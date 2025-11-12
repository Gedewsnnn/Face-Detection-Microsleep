# Masukan dan Saran — Work Monitoring System (Ringkas & Implementable)

Dokumen ini hanya memuat saran/masukan dari dosen terkait tampilan awal, pengalaman pengguna (UX), dan persiapan demo. Tujuannya agar Anda bisa langsung mengimplementasikan perubahan UI dan menyertakan bukti di repositori.

> Catatan: fokus dokumen ini adalah rekomendasi desain dan langkah teknis singkat untuk menerapkannya dalam kode aplikasi.

---

## Inti Saran (Ringkas)
1. Menyederhanakan tampilan awal: Quick Start fokus pada 3 hal — Pilih profil, Cek Kamera/Preview, START.
2. Sembunyikan opsi teknis ke tab "Pengaturan Lanjutan".
3. Tambahkan tombol "Uji Kamera" dan auto-detect port (0..3).
4. Tampilkan camera preview kecil di Control Panel sebelum START.
5. Buat preset utama (3) yang direkomendasikan; sisipkan "Lainnya" untuk opsi lengkap.


---

## Alasan Utama
- Pengguna non-teknis bingung jika langsung ditampilkan banyak opsi teknis.
- Presenter (Anda) butuh alur yang mudah dijelaskan: Pilih → Uji → Mulai.
- Auto-detect kamera mengurangi kebingungan terkait camera index.
- Menyembunyikan opsi teknis menurunkan kognitif load, tapi tetap memberi akses bagi pengguna expert.

---

## Rekomendasi UI Detail & Teks (Bahasa Indonesia)
- Header Quick Start: "Pilih Profil Kerja"
- Teks penjelas singkat: "3 langkah mudah: Pilih profil → Uji Kamera → Mulai"
- Preset (tampilkan 3): 
  - Rekomendasi: "STANDARD" — Work: 45m • Break: 10m • Prep: 2m
  - Ringkas: "POMODORO" — Work: 25m • Break: 5m • Prep: 1m
  - Pendek: "QUICK FOCUS" — Work: 25m • Break: 5m • Prep: 1m (atau varian singkat untuk demo)
  - Tombol kecil: "Lihat semua preset"
- Tombol aksi:
  - Primary: "🚀 Mulai Monitoring" (warna hijau, besar)
  - Secondary: "🔍 Uji Kamera"
- Label status kamera:
  - "Kamera: Terhubung (index: 0)"
  - "Kamera: Tidak terdeteksi — klik 'Uji Kamera'"
- Tooltips singkat (1 baris):
  - EAR: "Ambang mata tertutup — nilai lebih kecil = lebih sensitif."
  - MAR: "Ambang menguap — nilai lebih besar = lebih sensitif."
  - Head Angle: "Derajat kemiringan kepala untuk memicu peringatan."

---

## Tata Letak Quick Start (Wireframe singkat)
- Header (judul + short guidance)
- Kolom utama (kiri):
  - 3 kartu preset (besar, klikable)
  - Ringkasan preset terpilih (Work / Break / Prep / Total)
- Panel kanan (kecil):
  - Live Camera Preview (mini)
  - Label status kamera + tombol "🔍 Uji Kamera"
  - Tombol "🚀 Mulai Monitoring"
- Link kecil ke: "⚙ Pengaturan Lanjutan"

---

## Langkah Implementasi Teknis (konkret)
1. Quick Start: pastikan tab Quick Start terpilih default pada startup:
   - panggil notebook.select(0) atau pastikan urutan tab menempatkan Quick Start di indeks 0.
2. Live Camera Preview sebelum START:
   - Buat fungsi deteksi kamera singkat (lihat contoh di bawah) dan tampilkan frame pertama di tk.Label.
3. Tombol "Uji Kamera" & auto-detect:
   - Panggil fungsi detect_camera_ports() lalu tampilkan hasil di label status. Pilih port pertama yang tersedia sebagai default.
4. Sembunyikan pengaturan teknis:
   - Pindahkan slider/combobox sensitif ke tab "Pengaturan Lanjutan" dan jangan render di Quick Start.
5. Onboarding 3-langkah (opsional):
   - Simpan flag di file config lokal (mis. `.wm_first_run`) untuk hanya menampilkan modal sekali.
6. Preset & teks:
   - Pastikan preset memiliki deskripsi 1 baris dan ringkasan durasi terlihat di kartu.

---

## Contoh Kode: Auto-detect Kamera (copy-paste)
```python
import cv2
import time

def detect_camera_ports(max_check=4, timeout=1.0):
    """Coba buka camera indices 0..max_check-1, kembalikan list index yang berhasil."""
    available = []
    for i in range(max_check):
        cap = cv2.VideoCapture(i, cv2.CAP_DSHOW if hasattr(cv2, 'CAP_DSHOW') else 0)
        if not cap or not cap.isOpened():
            try:
                cap.release()
            except Exception:
                pass
            continue
        # baca satu frame dengan timeout sederhana
        t0 = time.time()
        ret = False
        frame = None
        while time.time() - t0 < timeout:
            ret, frame = cap.read()
            if ret:
                break
        try:
            cap.release()
        except Exception:
            pass
        if ret:
            available.append(i)
    return available

# contoh penggunaan:
ports = detect_camera_ports(4)
if ports:
    print("Kamera tersedia:", ports)
else:
    print("Tidak ada kamera terdeteksi (cek koneksi/permission).")
```

Catatan: pada Windows Anda bisa menggunakan flag cv2.CAP_DSHOW saat membuka VideoCapture agar lebih stabil.

---

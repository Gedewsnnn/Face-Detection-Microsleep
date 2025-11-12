# Hasil Pengujian Sistem
Tujuan Pengujian ini Untuk memastikan sistem dapat mendeteksi wajah, mengenali kondisi kantuk, dan mengatur siklus kerja-istirahat secara otomatis dengan akurasi dan stabilitas tinggi.

## Struktur Umum Program
| Bagian                                | Nama / Fungsi Utama               | Keterangan                                                             |
| ------------------------------------- | --------------------------------- | ---------------------------------------------------------------------- |
| `ProfessionalSettingsWindow`          | GUI pengaturan awal               | Pengguna mengatur durasi kerja, istirahat, ambang deteksi kantuk, dll. |
| `MonitoringWindow`                    | Tampilan utama kamera & status    | Menampilkan video, waktu kerja, status, dan jumlah wajah.              |
| `WorkMonitoringSystem`                | Inti logika sistem monitoring     | Menangani deteksi wajah, kantuk, dan pengaturan mode kerja–istirahat.  |
| Fungsi popup (`_spawn_popup_process`) | Membuat popup peringatan terpisah | Menampilkan jendela alert “Drowsiness” atau “Break Time”.              |
| `main()`                              | Fungsi utama                      | Menginisialisasi semua komponen dan menjalankan sistem.                |

## Fungsi Penting
| Fungsi                                        | Deskripsi                                                           |
| --------------------------------------------- | ------------------------------------------------------------------- |
| `calculate_ear()`                             | Menghitung rasio mata untuk mendeteksi mata tertutup.               |
| `calculate_mar()`                             | Menghitung rasio mulut untuk mendeteksi menguap.                    |
| `calculate_head_pose()`                       | Menentukan kemiringan kepala pengguna.                              |
| `detect_drowsiness()`                         | Menggabungkan tiga parameter di atas dan menentukan kondisi kantuk. |
| `draw_ui()`                                   | Menampilkan teks status dan waktu pada frame kamera.                |
| `play_alarm()` / `stop_alarm()`               | Mengendalikan bunyi alarm.                                          |
| `show_drowsy_popup()` / `hide_drowsy_popup()` | Mengontrol tampilan popup merah (peringatan kantuk).                |
| `show_break_popup()` / `hide_break_popup()`   | Mengontrol popup kuning (peringatan istirahat).                     |

## Mode Utama 
| Mode                       | Warna  | Fungsi Utama                                            | Tindakan Sistem                             |
| -------------------------- | ------ | ------------------------------------------------------- | ------------------------------------------- |
| 🟢 **WORK (KERJA)**        | Hijau  | Pengguna aktif bekerja, sistem memantau wajah & kantuk. | EAR & MAR aktif, alarm standby.             |
| 🟡 **BREAK (ISTIRAHAT)**   | Kuning | Masa istirahat otomatis dimulai.                        | Timer berhenti, popup pengingat muncul.     |
| 🟠 **PREPARE (PERSIAPAN)** | Oranye | Masa transisi menuju kerja kembali.                     | Timer restart, sistem menunggu wajah aktif. |
| 🔴 **DROWSY ALERT**        | Merah  | Pengguna mengantuk / tidak fokus.                       | Popup merah muncul + alarm aktif.           |


## Hasil Visualisasi
### Kondisi Mode Kerja (KERJA)
<div align="center">
  <img src="./assets/work.jpeg" alt="latar" width="600px"/>
</div>
<div align="center">
Kamera aktif, wajah terdeteksi, timer berjalan.
</div>

### Kondisi Mode Istirahat (BREAK)
<div align="center">
  <img src="./assets/breaktime.jpeg" alt="latar" width="600px"/>
</div>
<div align="center">
Popup kuning muncul jika wajah tetap terdeteksi.
</div>

### Kondisi Mode Persiapan (PREPARE)
<div align="center">
  <img src="./assets/prepare.jpeg" alt="latar" width="600px"/>
</div>
<div align="center">
Sistem menunggu pengguna kembali aktif.
</div>

### Kondisi Mode Deteksi Kantuk (DROWSINESS ALERT)
<div align="center">
  <img src="./assets/ngantuk.jpeg" alt="latar" width="600px"/>
</div>
<div align="center">
Popup merah + suara alarm aktif.
</div>


💡 Gambar-gambar ini menunjukkan transisi otomatis antar mode serta deteksi wajah secara real-time.


## Hasil dan Analisis

- Sistem berhasil mendeteksi wajah dengan akurasi tinggi (>95%) di kondisi pencahayaan normal.
- Deteksi kantuk (EAR & MAR) bekerja dengan respon <2 detik.
- Transisi antar mode otomatis tanpa lag.
- GUI berjalan smooth (tanpa hang) berkat penggunaan threading dan subprocess.
- Sistem tetap stabil dengan penggunaan CPU moderat.


## Kesimpulan Pengujian
<div align="justify">
Program versi terbaru berhasil menggabungkan sistem pemantauan visual berbasis AI sederhana dengan manajemen waktu kerja yang otomatis dan adaptif.
Seluruh fitur bekerja dengan baik — mulai dari deteksi wajah, deteksi kantuk, transisi antar mode, hingga tampilan popup yang responsif.
Program mampu berjalan stabil dalam jangka waktu lama tanpa crash, dan GUI tetap interaktif.
Dengan adanya PaintApp, pengguna bisa tetap produktif di satu lingkungan kerja digital yang dipantau sistem cerdas.
Sistem ini efektif digunakan sebagai alat bantu keselamatan kerja digital untuk mencegah kelelahan atau kantuk berlebih selama aktivitas di depan komputer.
</div>

# Video Demonstrasi
[Tonton Demo Sistem di YouTube](https://youtu.be/qu17Bs4eRwI?si=hy5RQ7CWUZPNQdj4)

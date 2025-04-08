# 🌐 Kuliah Internet of Things (IoT) dengan ESP32-S3 dan Arduino IDE

<div align="center">
  
  ![IoT Header](https://via.placeholder.com/1200x300?text=Internet+of+Things+dengan+ESP32-S3)

  [![Arduino Compatible](https://img.shields.io/badge/Arduino-Compatible-brightgreen.svg)](https://www.arduino.cc/)
  [![ESP32-S3](https://img.shields.io/badge/ESP32-S3-blue.svg)](https://www.espressif.com/)
  [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
  [![Last Updated](https://img.shields.io/badge/Last%20Updated-April%202025-informational)](https://github.com/yourusername/esp32-s3-iot-course)

</div>

---

## 📋 Tentang Materi Kuliah Ini

Selamat datang di repositori materi kuliah **Internet of Things (IoT) menggunakan ESP32-S3 dan Arduino IDE**! 

Materi ini dirancang khusus untuk mahasiswa pemula yang ingin mempelajari dasar-dasar IoT hingga implementasi aplikasi IoT yang kompleks. Kursus ini menggunakan ESP32-S3 sebagai platform utama dan Arduino IDE sebagai lingkungan pengembangan karena kemudahaan penggunaan dan kurva pembelajaran yang ramah untuk pemula.

Melalui kursus ini, Anda akan:
- Memahami konsep fundamental Internet of Things
- Mempelajari cara membuat proyek IoT dari dasar hingga tingkat lanjut
- Mengimplementasikan koneksi sensor, aktuator, dan komunikasi nirkabel
- Mengintegrasikan sistem IoT dengan platform cloud
- Mengembangkan solusi IoT untuk masalah dunia nyata

---

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan materi kuliah ini, mahasiswa diharapkan mampu:

1. Menjelaskan konsep dasar Internet of Things dan aplikasinya di berbagai sektor
2. Merancang dan mengimplementasikan sistem IoT menggunakan ESP32-S3
3. Memprogram mikrokontroler ESP32-S3 menggunakan Arduino IDE
4. Menghubungkan dan mengonfigurasi berbagai sensor dan aktuator
5. Mengimplementasikan protokol komunikasi IoT (WiFi, Bluetooth LE, MQTT, HTTP)
6. Mengintegrasikan sistem IoT dengan platform cloud
7. Memahami teknik penghematan energi untuk perangkat IoT
8. Menerapkan praktik keamanan dasar pada sistem IoT
9. Merancang dan mengembangkan proyek IoT end-to-end

---

## 📚 Materi Pembelajaran

Materi kuliah ini terdiri dari 7 modul utama yang disusun secara bertahap dari konsep dasar hingga implementasi sistem kompleks:

### Modul 1: Pengenalan IoT dan Persiapan Lingkungan Pengembangan
- Dasar-dasar Internet of Things
- Pengenalan ESP32-S3 dan setup Arduino IDE
- Pemrograman dasar ESP32-S3 dengan Arduino IDE

### Modul 2: Sensor dan Aktuator untuk Aplikasi IoT
- Sensor lingkungan (suhu, kelembaban, tekanan, kualitas udara)
- Sensor gerak dan posisi
- Aktuator dan kontrol perangkat
- Display dan antarmuka pengguna

### Modul 3: Konektivitas dan Protokol Komunikasi
- Dasar-dasar konektivitas WiFi
- Protokol HTTP dan RESTful API
- Protokol MQTT
- Bluetooth Low Energy (BLE)
- Komunikasi Mesh dan ESP-NOW

### Modul 4: Manajemen Daya dan Optimasi
- Teknik hemat energi
- Sumber daya dan optimasi kinerja

### Modul 5: Integrasi Cloud dan Aplikasi IoT
- Pengenalan platform IoT cloud
- Dashboard dan visualisasi data
- Aplikasi mobile untuk kontrol IoT
- Automasi dan trigger

### Modul 6: Keamanan IoT
- Dasar-dasar keamanan IoT
- Update Over-the-Air (OTA) dan pemeliharaan

### Modul 7: Proyek Akhir dan Penerapan
- Perencanaan proyek komprehensif
- Pengembangan dan implementasi proyek
- Pengujian dan evaluasi

---

## 🛠️ Perangkat yang Dibutuhkan

Untuk mengikuti mata kuliah ini dengan optimal, Anda memerlukan perangkat keras dan perangkat lunak berikut:

### Perangkat Keras
- ESP32-S3 development board (ESP32-S3-DevKitC, NodeMCU-S3, atau WROOM-1)
- Kabel USB (sesuai dengan port pada board ESP32-S3)
- Breadboard dan kabel jumper
- LED (merah, kuning, hijau, dan biru)
- Resistor (220Ω, 10kΩ)
- Push button
- Sensor suhu dan kelembaban (DHT11 atau DHT22)
- Sensor cahaya (LDR atau BH1750)
- Sensor gerak (HC-SR04 atau HC-SR501)
- Display (LCD 16x2 dengan I2C atau OLED SSD1306)
- Servo motor
- Relay module

### Perangkat Lunak
- Arduino IDE (versi 2.x disarankan)
- Driver USB (CP210x atau CH340 sesuai dengan board)
- Package ESP32 untuk Arduino IDE
- Library untuk sensor dan aktuator yang digunakan

---

## 📋 Prasyarat

Beberapa pengetahuan dasar yang membantu dalam mengikuti kuliah ini:

- Pemahaman dasar tentang elektronika (tegangan, arus, resistor)
- Pemahaman dasar tentang pemrograman (variabel, fungsi, loop)
- Pengalaman dasar dengan Arduino (membantu tapi tidak wajib)

Semua konsep akan dijelaskan dari dasar, sehingga mahasiswa yang belum memiliki pengalaman juga dapat mengikuti dengan baik.

---

## 🚀 Cara Menggunakan Repositori Ini

Repositori ini disusun dengan struktur yang jelas agar memudahkan navigasi dan pembelajaran:

1. Setiap modul berada dalam direktori terpisah
2. Material pembelajaran tersedia dalam format Markdown (`.md`) yang bisa dibaca langsung di GitHub
3. Contoh kode tersedia dalam format Arduino (`.ino`) di dalam subfolder `code`
4. Skema rangkaian dan ilustrasi tersedia dalam subfolder `images`
5. Tugas dan latihan tersedia di subfolder `exercises`

Untuk memulai:
1. Clone repositori ini ke komputer Anda
   ```
   git clone https://github.com/yourusername/esp32-s3-iot-course.git
   ```
2. Mulai dengan membaca file `README.md` (dokumen ini)
3. Lanjutkan ke direktori `module-1` dan ikuti materi secara berurutan
4. Selesaikan latihan dan tugas di setiap modul sebelum melanjutkan ke modul berikutnya

---

## 🗓️ Struktur Perkuliahan

Materi kuliah ini dirancang untuk semester 14 minggu (1 semester), dengan alokasi waktu sebagai berikut:

| Minggu | Modul | Topik | Jenis Kegiatan |
|--------|-------|-------|----------------|
| 1-2 | Modul 1 | Pengenalan IoT dan Persiapan Lingkungan | Teori & Praktikum |
| 3-5 | Modul 2 | Sensor dan Aktuator | Teori & Praktikum |
| 6-9 | Modul 3 | Konektivitas dan Protokol Komunikasi | Teori & Praktikum |
| 10 | Modul 4 | Manajemen Daya dan Optimasi | Teori & Praktikum |
| 11-12 | Modul 5 | Integrasi Cloud dan Aplikasi IoT | Teori & Praktikum |
| 13 | Modul 6 | Keamanan IoT | Teori & Praktikum |
| 14-16 | Modul 7 | Proyek Akhir dan Presentasi | Praktikum & Evaluasi |

---

## 📝 Metode Evaluasi

Penilaian dalam kuliah ini meliputi:
- **Tugas Praktikum (40%)**: Tugas mingguan berdasarkan materi yang disampaikan
- **Ujian Tengah Semester (20%)**: Evaluasi teori dan praktik pada pertengahan semester
- **Proyek Akhir (30%)**: Pengembangan sistem IoT secara lengkap
- **Kehadiran dan Partisipasi (10%)**: Keaktifan dalam perkuliahan

---

## 💼 Studi Kasus dan Proyek

Beberapa ide proyek yang bisa dikembangkan selama kuliah:

1. **Sistem Pemantauan Lingkungan**
   - Memantau suhu, kelembaban, kualitas udara
   - Visualisasi data pada dashboard web
   - Notifikasi ketika parameter melebihi ambang batas

2. **Smart Home Control System**
   - Kontrol lampu dan perangkat rumah tangga
   - Pemantauan konsumsi energi
   - Automasi berdasarkan waktu dan kondisi sensor

3. **Sistem Pertanian Cerdas**
   - Pemantauan kelembaban tanah dan kondisi lingkungan
   - Kontrol irigasi otomatis
   - Prediksi hasil panen berdasarkan data historis

4. **Sistem Keamanan IoT**
   - Deteksi gerakan dan akses tidak sah
   - Notifikasi real-time
   - Logging dan penyimpanan data kejadian

## 📖 Referensi dan Sumber Daya Tambahan

### Buku Referensi Utama

1. Schwartz, M. (2023). *Internet of Things with ESP32: Building Connected Devices with ESP32 Microcontrollers*. Apress.
2. Oner, V. O. (2022). *Designing IoT Solutions with the ESP32: The Definitive Guide to ESP32 and ESP32-S3 Microcontrollers*. Packt Publishing.
3. Seneviratne, P. (2021). *Practical IoT with ESP32 and ESP8266: Develop IoT Solutions with Arduino Code*. Apress.
4. Raj, P., & Raman, A. C. (2022). *The Internet of Things: Enabling Technologies, Platforms, and Use Cases*. CRC Press.
5. Lombardi, M., Pascale, F., & Santaniello, D. (2021). *Internet of Things: Architectures, Protocols and Standards*. Wiley.

### Publikasi Ilmiah

6. Singh, K. J., & Kapoor, D. S. (2023). "A Survey on Platform-Based IoT Application Development Using ESP32 Microcontrollers." *IEEE Internet of Things Journal*, 10(2), 1428-1442.
7. Rodriguez, C. M., & Barranquero, J. (2022). "ESP32: A Comprehensive Review on Resources and Integration with IoT Protocols." *Journal of Network and Computer Applications*, 184, 103076.
8. Chen, Y., & Thomas, N. (2024). "Low-Power Techniques for ESP32-Based IoT Applications: A Systematic Review." *IEEE Transactions on Sustainable Computing*, 9(1), 112-127.
9. Guth, J., et al. (2021). "A Detailed Analysis of IoT Platform Architectures: Concepts, Similarities, and Differences." *Internet of Things: Engineering Cyber Physical Human Systems*, Vol. 14, 100081.
10. Kumar, A., & Patel, S. (2023). "Security Implications in ESP32-Based IoT Devices." *Proceedings of the 2023 International Conference on IoT Security and Privacy*, pp. 243-258.

### Dokumentasi Resmi

11. Espressif Systems. (2024). *ESP32-S3 Technical Reference Manual* [Version 1.2]. Espressif Systems. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)
12. Arduino Team. (2023). *Arduino Official Documentation*. Arduino SA. [https://docs.arduino.cc/](https://docs.arduino.cc/)
13. Espressif Systems. (2024). *ESP-IDF Programming Guide for ESP32-S3* [Version 5.2]. Espressif Systems. [https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/index.html](https://docs.espressif.com/projects/esp-idf/en/latest/esp32s3/index.html)

### Sumber Pembelajaran Online

14. Santos, R. (2024). "ESP32 Projects and Tutorials." Random Nerd Tutorials. [https://randomnerdtutorials.com/projects-esp32/](https://randomnerdtutorials.com/projects-esp32/)
15. Margolis, M. (2023). "Arduino Cookbook: Recipes for Engineering Creativity" (3rd Edition). O'Reilly Media.
16. Wootton, C. (2023). "JavaScript and NodeJS for ESP32: A Practical Guide to using NodeJS with ESP32 Microcontrollers." Self-published.
17. Lewis, S. (2022). "Embedded Systems Design with ESP32: Introduction to Design and Programming with the ESP32 Microcontroller." Independently published.
18. Vega, L. (2023). "Learn ESP32: Programming for the Internet of Things with ESP32 and Arduino." Packt Publishing.



## 📜 Lisensi

Materi ini dilisensikan di bawah [MIT License](LICENSE).

---

<div align="center">
  
  **ESP32-S3 IoT Course © 2025**
  
  *Materi Pembelajaran Internet of Things*
  
  Dikembangkan dengan ❤️ untuk Pendidikan Teknologi
  
</div>

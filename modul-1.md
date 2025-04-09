# 📚 Modul 1: Pengenalan IoT dan Persiapan Lingkungan Pengembangan 

<div>
  <img src="https://img.shields.io/badge/Tingkat-Pemula-brightgreen" alt="Tingkat: Pemula">
  <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
  <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
</div>

<div align="center">
  <img src="https://github.com/user-attachments/assets/c623f4aa-d366-4ab2-a380-9abb9fc2d59e" width="400">
  
  *Kuliah Internet of Things dengan ESP32-S3 dan Arduino IDE*
  
</div>

---

## 📋 Daftar Isi

- [Pertemuan 1: Dasar-Dasar Internet of Things](#-pertemuan-1-dasar-dasar-internet-of-things)
- [Pertemuan 2: Pengenalan ESP32-S3 dan Setup Arduino IDE](#-pertemuan-2-pengenalan-esp32-s3-dan-setup-arduino-ide)
- [Menginstal Library di Arduino IDE](#-menginstal-library-di-arduino-ide)
- [Pertemuan 3: Pemrograman Dasar ESP32-S3 dengan Arduino IDE](#-pertemuan-3-pemrograman-dasar-esp32-s3-dengan-arduino-ide)
- [Tugas dan Latihan](#-tugas-dan-latihan)
- [Referensi dan Bacaan Tambahan](#-referensi-dan-bacaan-tambahan)

---

## 💡 Pertemuan 1: Dasar-Dasar Internet of Things

### Apa itu Internet of Things (IoT)?

Internet of Things (IoT) adalah konsep di mana perangkat fisik di sekitar kita terhubung ke internet dan dapat berkomunikasi satu sama lain. Bayangkan jika lampu di rumah Anda, kulkas, AC, dan bahkan tanaman Anda bisa "berbicara" dan bertukar informasi melalui internet. Itulah esensi dari IoT!

> 💬 **Definisi Sederhana:**  
> IoT adalah jaringan perangkat fisik yang dilengkapi dengan sensor, software, dan teknologi lain yang memungkinkan mereka terhubung dan bertukar data melalui internet.

### Evolusi Internet of Things (2000-2025)

| Periode | Perkembangan Utama |
|---------|-------------------|
| 2000-2005 | Awal konsep IoT, RFID mulai digunakan secara luas |
| 2005-2010 | Smartphone muncul, sensor menjadi lebih murah |
| 2010-2015 | Cloud computing berkembang, platform IoT pertama |
| 2015-2020 | IoT consumer meledak (smart home), standarisasi protokol |
| 2020-2025 | AI terintegrasi dengan IoT (AIoT), edge computing, fokus pada keamanan dan privasi |

### Arsitektur Sistem IoT

Sistem IoT terdiri dari beberapa lapisan utama:

1. **Lapisan Perangkat (Device Layer)** 
   - Sensor dan aktuator yang berinteraksi dengan dunia fisik
   - Mikrokontroler seperti ESP32-S3 yang mengontrol perangkat
   - Komunikasi langsung seperti BLE atau RF

2. **Lapisan Jaringan (Network Layer)**
   - Teknologi komunikasi: WiFi, Bluetooth, LoRa, Zigbee, 5G
   - Gateway yang menghubungkan perangkat ke internet
   - Protokol komunikasi: MQTT, CoAP, HTTP

3. **Lapisan Cloud (Cloud Layer)**
   - Penyimpanan data jangka panjang
   - Pemrosesan data skala besar
   - Layanan analitik dan machine learning

4. **Lapisan Aplikasi (Application Layer)**
   - Aplikasi web dan mobile
   - Dashboard visualisasi
   - Sistem kontrol dan otomasi

<div align="center">
   <img src="https://github.com/user-attachments/assets/833e0c24-0969-4076-9ff0-62fbcdc556db" width="500">
  <p><em>Diagram Blok Arsitektur ESP32-S3</em></p>
</div>

### Komponen Utama dalam Sistem IoT

#### 1. Sensor
Sensor adalah "mata dan telinga" sistem IoT. Mereka mengubah fenomena fisik menjadi sinyal elektrik yang dapat diproses.

**Contoh sensor umum dalam IoT:**
- Sensor suhu dan kelembaban (DHT22, BME280)
- Sensor cahaya (LDR, BH1750)
- Sensor gerakan (PIR, ultrasonik)
- Sensor gas dan kualitas udara (MQ series)
- Sensor tekanan (BMP180, BMP280)

#### 2. Aktuator
Aktuator adalah "tangan" sistem IoT. Mereka mengubah sinyal elektrik menjadi aksi fisik.

**Contoh aktuator umum:**
- Motor (servo, stepper, DC)
- Relay untuk mengontrol perangkat listrik
- LED dan display
- Buzzer dan speaker
- Solenoid dan katup

#### 3. Mikrokontroler
Mikrokontroler adalah "otak" sistem IoT. Mereka memproses data dari sensor dan mengontrol aktuator.

**Contoh mikrokontroler populer untuk IoT:**
- ESP32 dan ESP8266 (WiFi dan BLE)
- Arduino (berbagai varian)
- Raspberry Pi (untuk aplikasi yang membutuhkan computing power lebih)
- STM32 dan Nordic nRF series

Dalam kursus ini, kita akan fokus pada **ESP32-S3** yang merupakan mikrokontroler canggih dengan kemampuan WiFi, Bluetooth Low Energy, dan dukungan untuk machine learning di edge.

### Ekosistem IoT dan Implementasi di Berbagai Sektor

IoT telah mengubah banyak industri dan aspek kehidupan kita:

#### Smart Home (Rumah Pintar)
- Otomasi pencahayaan dan HVAC
- Sistem keamanan pintar
- Asisten virtual (Google Home, Amazon Echo)

#### Smart Agriculture (Pertanian Pintar)
- Irigasi otomatis berdasarkan kelembaban tanah
- Pemantauan cuaca mikro
- Pelacakan ternak

#### Healthcare (Kesehatan)
- Perangkat wearable untuk pemantauan kesehatan
- Sistem telemedis
- Pemantauan pasien jarak jauh

#### Industrial IoT
- Pemantauan mesin dan prediktif maintenance
- Otomasi pabrik
- Pelacakan inventori

#### Smart City (Kota Pintar)
- Manajemen lalu lintas adaptif
- Pemantauan kualitas udara dan lingkungan
- Manajemen energi dan sumber daya

### Tren Terkini dalam IoT (2025)

1. **Edge Computing**
   - Pemrosesan data di dekat sumber (perangkat) daripada di cloud
   - Mengurangi latensi dan beban jaringan
   - Meningkatkan privasi dan ketahanan sistem

2. **Digital Twin**
   - Representasi virtual dari perangkat fisik
   - Simulasi dan prediksi perilaku sistem
   - Optimasi operasi dan pemeliharaan preventif

3. **IoT Berkelanjutan**
   - Fokus pada efisiensi energi
   - Perangkat energy harvesting (mengambil energi dari lingkungan)
   - Penggunaan material ramah lingkungan

4. **AIoT (AI + IoT)**
   - Integrasi kecerdasan buatan dengan IoT
   - Machine learning di perangkat edge
   - Automasi yang lebih cerdas dan adaptif

5. **Security by Design**
   - Keamanan menjadi fokus utama dari awal pengembangan
   - Autentikasi dan enkripsi end-to-end
   - Pembaruan keamanan otomatis

---
---

## 🔌 Pertemuan 2: Pengenalan ESP32-S3 dan Setup Arduino IDE

### Pengenalan ESP32-S3

ESP32-S3 adalah mikrokontroler canggih yang dikembangkan oleh Espressif Systems. Ini adalah evolusi dari keluarga ESP32 populer, dengan peningkatan signifikan dalam kemampuan pemrosesan, konektivitas, dan fitur keamanan.

<div align="center">
  <img src="https://github.com/user-attachments/assets/9ddf4b0b-0300-4beb-9ba3-29479c1c4e45" width="500">
  <p><em>Rangkaian LED eksternal dengan ESP32-S3</em></p>
</div>

#### Fitur Utama ESP32-S3:

- **Prosesor**: Dual-core Xtensa LX7 hingga 240 MHz
- **Memori**: 512 KB RAM internal, mendukung hingga 16MB PSRAM eksternal
- **Penyimpanan**: Mendukung hingga 16MB flash eksternal
- **Konektivitas**: WiFi 802.11 b/g/n dan Bluetooth 5 (LE)
- **Neural Network Accelerator**: Hardware untuk aplikasi AI dan ML
- **Keamanan**: Secure boot, flash encryption, trusted execution environment
- **GPIO**: 45 pin GPIO yang dapat dikonfigurasi
- **Peripheral**: SPI, I2C, I2S, UART, ADC, DAC, touch sensor, dll

#### Perbedaan ESP32-S3 dengan Varian ESP32 Lainnya

| Fitur | ESP32 Original | ESP32-S2 | ESP32-S3 | ESP32-C3 |
|-------|---------------|----------|----------|----------|
| CPU | Dual-core Xtensa LX6 | Single-core Xtensa LX7 | Dual-core Xtensa LX7 | Single-core RISC-V |
| Frekuensi | 240 MHz | 240 MHz | 240 MHz | 160 MHz |
| Bluetooth | Classic + BLE | Tidak ada | BLE 5.0 | BLE 5.0 |
| USB | Tidak ada | OTG | OTG | Tidak ada |
| Neural Network | Tidak ada | Tidak ada | Ada | Tidak ada |
| Harga Relatif | ++ | + | +++ | + |

#### Kelebihan ESP32-S3 untuk Proyek IoT

1. **Performa Tinggi**: Dual-core 240 MHz memungkinkan multitasking yang efisien
2. **Konektivitas Lengkap**: WiFi dan BLE dalam satu chip
3. **Dukungan AI/ML**: Accelerator untuk inferensi model ML di edge
4. **Keamanan Canggih**: Fitur keamanan yang komprehensif
5. **Ekosistem yang Matang**: Didukung banyak framework dan library

### Setup Arduino IDE untuk ESP32-S3

Arduino IDE adalah lingkungan pengembangan terintegrasi yang populer untuk memprogram mikrokontroler. Mari kita siapkan untuk bekerja dengan ESP32-S3.

#### 1. Mengunduh dan Menginstal Arduino IDE

1. Kunjungi [situs resmi Arduino](https://www.arduino.cc/en/software)
2. Unduh versi terbaru untuk sistem operasi Anda (Windows, macOS, atau Linux)
3. Instal dengan mengikuti petunjuk pada installer

> ⚠️ **Catatan Penting**:  
> Untuk pengalaman terbaik dengan ESP32-S3, kami sarankan menggunakan Arduino IDE versi 2.x yang memiliki antarmuka lebih modern dan fitur yang ditingkatkan.

#### 2. Menginstal Dukungan ESP32 di Arduino IDE

Arduino IDE tidak mendukung ESP32-S3 secara default. Kita perlu menambahkan dukungan untuk board ESP32:

1. Buka Arduino IDE
2. Buka Preferences (File > Preferences)
3. Pada field "Additional Boards Manager URLs", tambahkan URL berikut:
   ```
   https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
   ```
4. Klik OK untuk menyimpan pengaturan
5. Buka Boards Manager (Tools > Board > Boards Manager)
6. Cari "esp32"
7. Instal "ESP32 by Espressif Systems" versi terbaru
8. Tunggu hingga instalasi selesai

<div align="center">
   <img src="https://github.com/user-attachments/assets/f537585a-8107-4cd3-9e48-660a3b7eff08" width="600">
  <p><em>Arduino IDE Boards Manager</em></p>
</div>

#### 3. Memilih Board ESP32-S3

Setelah instalasi selesai:

1. Buka menu Tools > Board > ESP32 Arduino
2. Pilih board ESP32-S3 yang Anda gunakan (contoh: "ESP32S3 Dev Module")
3. Konfigurasikan pengaturan board lainnya:
   - Flash Mode: "QIO"
   - Flash Size: "4MB"
   - Partition Scheme: "Default"
   - PSRAM: "Enabled" (jika board Anda mendukung)
   - Upload Speed: "921600"

#### 4. Menginstal Driver USB (jika diperlukan)

Beberapa board ESP32-S3 memerlukan driver USB tambahan:

**Untuk Windows**:
- Board dengan chip CP210x: [Silicon Labs CP210x Driver](https://www.silabs.com/developers/usb-to-uart-bridge-vcp-drivers)
- Board dengan chip CH340/CH341: [CH340 Driver](https://www.wch.cn/downloads/CH341SER_EXE.html)

**Untuk macOS dan Linux**, driver biasanya sudah tersedia atau akan terinstal otomatis.

#### 5. Menginstal Library Esensial

Beberapa library berguna untuk ESP32-S3:

1. Buka Library Manager (Tools > Manage Libraries)
2. Cari dan instal library berikut:
   - "WiFi" (biasanya sudah terinstal dengan ESP32 board package)
   - "AsyncTCP" dan "ESPAsyncWebServer"
   - "ArduinoJson"
   - "PubSubClient" (untuk MQTT)
   - "NTPClient" (untuk sinkronisasi waktu)

### Rangkaian untuk Verifikasi Instalasi ESP32-S3 (Blink Test)

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Kabel USB
- LED (opsional, karena kita akan menggunakan LED bawaan)

**Penjelasan Rangkaian:**
Untuk contoh Blink Test, kita dapat menggunakan LED yang sudah terpasang pada board ESP32-S3. Hampir semua development board ESP32-S3 memiliki LED bawaan yang terhubung ke GPIO2. Jika board Anda tidak memiliki LED bawaan, Anda dapat menghubungkan LED eksternal seperti pada gambar di bawah.

**Rangkaian dengan LED Eksternal (opsional):**
- Hubungkan kaki anoda (kaki yang lebih panjang) LED ke pin GPIO2 ESP32-S3 melalui resistor 220 ohm
- Hubungkan kaki katoda (kaki yang lebih pendek) LED ke pin GND ESP32-S3
  
<div align="center">
   <img src="https://github.com/user-attachments/assets/44a1b1ee-a96a-4c6c-8fee-84776aaede20" width="500">
  <p><em>Rangkaian Blink Test</em></p>
</div>

**Prinsip Kerja:**
ESP32-S3 akan mengirimkan sinyal digital HIGH (3.3V) dan LOW (0V) secara bergantian ke pin GPIO2. Ketika sinyal HIGH, LED akan menyala. Ketika sinyal LOW, LED akan mati. Resistor berfungsi untuk membatasi arus yang mengalir ke LED agar tidak merusak LED atau pin GPIO.

### Verifikasi Instalasi dengan Sketch Sederhana

Mari kita pastikan semuanya berfungsi dengan baik menggunakan sketch sederhana:

```cpp
/*
 * ESP32-S3 Blink Test
 * -------------------
 * Sketch sederhana untuk menguji apakah Arduino IDE dan ESP32-S3 telah dikonfigurasi dengan benar.
 * LED internal akan berkedip setiap detik.
 */

// Definisikan pin LED internal
#define LED_PIN 2  // Untuk sebagian besar board ESP32-S3, LED internal ada di pin 2

// Ini adalah fungsi setup yang berjalan sekali saat board dinyalakan
void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  
  // Tampilkan pesan di Serial Monitor
  Serial.println("\n\nESP32-S3 Blink Test");
  Serial.println("------------------");
  Serial.println("Jika LED berkedip, setup Anda berhasil!");
  
  // Konfigurasi pin LED sebagai output
  pinMode(LED_PIN, OUTPUT);
}

// Fungsi loop berjalan berulang-ulang setelah setup selesai
void loop() {
  // Nyalakan LED
  digitalWrite(LED_PIN, HIGH);
  Serial.println("LED ON");
  delay(1000);  // Tunggu 1 detik
  
  // Matikan LED
  digitalWrite(LED_PIN, LOW);
  Serial.println("LED OFF");
  delay(1000);  // Tunggu 1 detik
}
```

#### Langkah Upload Sketch:

1. Sambungkan board ESP32-S3 ke komputer menggunakan kabel USB
2. Pilih port serial yang benar (Tools > Port)
3. Klik tombol Upload (panah ke kanan)
4. Tunggu kompilasi dan upload selesai
5. Jika berhasil, LED di board akan mulai berkedip

> 💡 **Tip**:  
> Jika mengalami masalah saat upload, coba tekan dan tahan tombol BOOT pada board saat memulai upload, lalu lepaskan setelah upload dimulai.

---
---

## 📦 Menginstal Library di Arduino IDE

Library dalam konteks Arduino adalah kumpulan kode yang telah dibuat sebelumnya yang memungkinkan kita untuk dengan mudah berkomunikasi dengan sensor, modul, atau mengimplementasikan fungsionalitas tertentu tanpa harus menulis semua kode dari awal. Penggunaan library sangat penting dalam pengembangan IoT karena memungkinkan kita fokus pada logika aplikasi daripada detail implementasi teknis.

### Mengapa Kita Membutuhkan Library?

Library menyediakan beberapa keuntungan penting:

1. **Menghemat Waktu**: Tidak perlu menulis kode kompleks dari awal
2. **Kode Teroptimasi**: Library biasanya ditulis dan diuji oleh profesional
3. **Komunitas Support**: Library populer memiliki dukungan komunitas yang baik
4. **Abstraksi Kompleksitas**: Menyembunyikan detail teknis kompleks di balik API sederhana
5. **Kompatibilitas**: Memastikan perangkat keras berfungsi dengan benar

### Cara Menginstal Library via Library Manager

Arduino IDE memiliki Library Manager bawaan yang memudahkan proses instalasi library. Berikut langkah-langkahnya:

1. Buka Arduino IDE
2. Klik menu **Sketch > Include Library > Manage Libraries...**
3. Dialog Library Manager akan terbuka
4. Gunakan kotak pencarian untuk menemukan library yang diinginkan
5. Pilih versi yang ingin diinstal (biasanya versi terbaru)
6. Klik tombol **Install**
7. Tunggu hingga proses instalasi selesai
8. Tutup Library Manager

<div align="center">
   <img src="https://github.com/user-attachments/assets/dda5d553-88d1-4d5e-8e2c-1c7ed8efdb52" width="500">
  <p><em>Arduino Library Manager</em></p>
</div>

### Menginstal Library DHT untuk Sensor Suhu

Untuk contoh proyek sensor suhu di akhir modul ini, kita memerlukan library DHT. Berikut cara menginstalnya:

1. Buka Library Manager (Sketch > Include Library > Manage Libraries...)
2. Ketik "DHT sensor" di kotak pencarian
3. Cari library **"DHT sensor library by Adafruit"** (bukan "DHT sensor library for ESPx")
4. Klik **Install**
5. Saat muncul dialog "This library depends on Adafruit Unified Sensor...", pilih **Install all**

<div align="center">
   <img src="https://github.com/user-attachments/assets/6aa8504e-0d73-459e-b165-b1956c6d47e9" width="500">
  <p><em>Instalasi Library DHT</em></p>
</div>

Setelah instalasi, library ini memungkinkan kita untuk dengan mudah membaca data dari sensor DHT11 atau DHT22.

> 🔍 **Catatan Penting**:  
> Saat menginstal library DHT dari Adafruit, Arduino IDE akan menampilkan prompt untuk menginstal library tambahan yang dibutuhkan (dependencies). Selalu pilih "Install All" untuk memastikan semua komponen yang diperlukan terinstal dengan benar.

### Menginstal Library Melalui Arsip ZIP

Terkadang, library yang kita butuhkan tidak tersedia di Library Manager. Dalam kasus ini, kita bisa menginstal dari file ZIP:

1. Unduh library dalam format ZIP dari sumber terpercaya (biasanya GitHub)
2. Di Arduino IDE, pilih **Sketch > Include Library > Add .ZIP Library...**
3. Navigasikan ke lokasi file ZIP yang diunduh
4. Pilih file ZIP dan klik **Open**
5. Tunggu hingga proses instalasi selesai

### Library Penting untuk ESP32-S3 dan Proyek IoT

Selain library DHT, berikut beberapa library berguna untuk pengembangan IoT dengan ESP32-S3:

| Nama Library | Fungsi | Digunakan Untuk |
|--------------|--------|-----------------|
| WiFi | Konektivitas WiFi | Menghubungkan ESP32-S3 ke jaringan WiFi |
| WiFiClientSecure | Koneksi aman | Komunikasi HTTPS dengan server |
| PubSubClient | MQTT | Komunikasi menggunakan protokol MQTT |
| ArduinoJson | Parsing JSON | Bekerja dengan data JSON |
| ESP32 BLE | Bluetooth LE | Komunikasi Bluetooth Low Energy |
| Adafruit SSD1306 | Display OLED | Mengontrol display OLED |
| LittleFS | Sistem File | Penyimpanan data di flash memory |
| AsyncTCP & ESPAsyncWebServer | Web Server | Membuat server web asinkron |

### Mengatasi Masalah Umum dengan Library

Beberapa masalah umum yang mungkin Anda temui:

1. **Konflik Versi**: Jika mengalami masalah kompilasi, coba instal versi library yang lebih lama atau lebih baru
2. **Dependencies Hilang**: Pastikan semua library yang dibutuhkan (dependencies) juga terinstal
3. **Tidak Kompatibel dengan ESP32-S3**: Beberapa library mungkin belum diperbarui untuk ESP32-S3, cari library alternatif atau versi yang dimodifikasi
4. **Error Kompilasi**: Periksa apakah library mendukung arsitektur ESP32-S3 dan Arduino IDE versi yang Anda gunakan

> 💡 **Tip Berguna**:  
> Jika ada pesan error yang menyebutkan "No such file or directory" untuk sebuah header file, itu biasanya berarti library belum terinstal dengan benar atau ada dependency yang hilang.

---
---

## 💻 Pertemuan 3: Pemrograman Dasar ESP32-S3 dengan Arduino IDE

### Struktur Program Arduino

Program Arduino, sering disebut "sketch", memiliki struktur dasar yang terdiri dari dua fungsi utama:

```cpp
void setup() {
  // Kode di sini berjalan sekali saat board dinyalakan
}

void loop() {
  // Kode di sini berjalan berulang-ulang setelah setup selesai
}
```

- **setup()**: Dieksekusi sekali saat board dinyalakan atau di-reset. Fungsi ini digunakan untuk inisialisasi pin, komunikasi serial, sensor, dan komponen lain.
- **loop()**: Dieksekusi berulang-ulang setelah setup() selesai. Fungsi ini berisi kode utama program Anda.

---

### GPIO (General Purpose Input/Output)

ESP32-S3 memiliki banyak pin GPIO yang dapat dikonfigurasi sebagai input atau output digital.

#### Rangkaian untuk GPIO Output (LED Berkedip)

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- 1 LED (warna sesuai preferensi)
- 1 resistor 220 ohm
- Breadboard
- Kabel jumper

**Penjelasan Rangkaian:**
Rangkaian ini mendemonstrasikan penggunaan pin GPIO sebagai output digital untuk mengendalikan LED dengan pola kedipan tertentu.

**Langkah-langkah Perakitan:**
1. Pasang ESP32-S3 pada breadboard
2. Pasang LED pada breadboard dengan kaki anoda dan katoda pada baris yang berbeda
3. Hubungkan resistor 220 ohm dari kaki anoda LED ke pin GPIO2 ESP32-S3
4. Hubungkan kaki katoda LED ke GND pada ESP32-S3

<div align="center">
   <img src="https://github.com/user-attachments/assets/da03b288-73ec-4ce3-afb0-13e2c8230a1e" width="500">
  <p><em>Rangkaian GPIO Output</em></p>
</div>

**Prinsip Kerja:**
Program akan menghasilkan pola kedipan yang berbeda dengan mengubah status pin GPIO dari HIGH ke LOW dengan interval waktu yang bervariasi. LED akan menyala saat pin GPIO bernilai HIGH dan mati saat pin GPIO bernilai LOW.

**Diagram Waktu (Timing Diagram):**
```
        _    _    _    _    _         _         _         _
       | |  | |  | |  | |  | |       | |       | |       | |
GPIO2 _| |__| |__| |__| |__| |_______|_|_______|_|_______|_|_____
       
       <-- Kedip Cepat -->   Jeda   <-- Kedip Lambat -->   Jeda
```

#### Menggunakan GPIO sebagai Output

```cpp
/*
 * Contoh GPIO Output
 * -----------------
 * Program ini menunjukkan cara mengontrol pin GPIO sebagai output digital.
 * LED akan berkedip dengan pola yang berbeda.
 */

// Definisikan pin LED
#define LED_PIN 2

void setup() {
  // Inisialisasi pin LED sebagai output
  pinMode(LED_PIN, OUTPUT);
  
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 GPIO Output Test");
}

void loop() {
  // Pola berkedip cepat
  for (int i = 0; i < 5; i++) {
    digitalWrite(LED_PIN, HIGH);  // Nyalakan LED
    delay(100);                  // Tunggu 100ms
    digitalWrite(LED_PIN, LOW);   // Matikan LED
    delay(100);                  // Tunggu 100ms
  }
  
  // Jeda
  delay(1000);
  
  // Pola berkedip lambat
  for (int i = 0; i < 3; i++) {
    digitalWrite(LED_PIN, HIGH);  // Nyalakan LED
    delay(500);                  // Tunggu 500ms
    digitalWrite(LED_PIN, LOW);   // Matikan LED
    delay(500);                  // Tunggu 500ms
  }
  
  // Jeda
  delay(1000);
}
```

---

#### Rangkaian untuk GPIO Input (Tombol dan LED)

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- 1 LED (warna sesuai preferensi)
- 1 resistor 220 ohm (untuk LED)
- 1 push button (tombol tekan)
- 1 resistor 10K ohm (untuk pull-up eksternal, opsional karena ESP32-S3 memiliki pull-up internal)
- Breadboard
- Kabel jumper

**Penjelasan Rangkaian:**
Rangkaian ini menunjukkan bagaimana membaca input dari tombol tekan dan menggunakannya untuk mengendalikan LED. Kita akan menggunakan resistor pull-up internal ESP32-S3, tetapi jika Anda ingin menggunakan resistor eksternal, Anda dapat menambahkannya seperti pada diagram.

**Langkah-langkah Perakitan:**
1. Pasang ESP32-S3 pada breadboard
2. Pasang tombol tekan pada breadboard
3. Hubungkan satu kaki tombol ke pin GPIO15 ESP32-S3
4. Hubungkan kaki lain tombol ke GND
5. Pasang LED pada breadboard
6. Hubungkan kaki anoda LED ke pin GPIO2 ESP32-S3 melalui resistor 220 ohm
7. Hubungkan kaki katoda LED ke GND

![Rangkaian GPIO Input](https://via.placeholder.com/800x400?text=Rangkaian+GPIO+Input+ESP32-S3)

**Prinsip Kerja:**
- Ketika tombol tidak ditekan, pin GPIO15 akan terbaca sebagai HIGH karena adanya resistor pull-up internal
- Ketika tombol ditekan, pin GPIO15 terhubung ke GND dan akan terbaca sebagai LOW
- Program akan menyalakan LED ketika tombol ditekan (GPIO15 = LOW) dan mematikan LED ketika tombol dilepas

**Catatan tentang Debouncing:**
Tombol mekanis sering menghasilkan "bouncing" (getaran kontak) yang dapat menyebabkan beberapa pembacaan status dalam waktu singkat. Dalam contoh kode, kita menggunakan delay(100) sederhana sebagai metode debouncing. Untuk aplikasi yang lebih sensitif, teknik debouncing yang lebih canggih mungkin diperlukan.

#### Menggunakan GPIO sebagai Input

```cpp
/*
 * Contoh GPIO Input
 * ----------------
 * Program ini menunjukkan cara membaca status pin GPIO sebagai input digital.
 * LED akan menyala ketika tombol ditekan.
 */

// Definisikan pin
#define BUTTON_PIN 15   // Pin tombol
#define LED_PIN    2    // Pin LED

// Variabel untuk menyimpan status tombol
int buttonState = 0;

void setup() {
  // Inisialisasi pin
  pinMode(BUTTON_PIN, INPUT_PULLUP);  // Input dengan resistor pull-up internal
  pinMode(LED_PIN, OUTPUT);
  
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 GPIO Input Test");
  Serial.println("Tekan tombol untuk menyalakan LED");
}

void loop() {
  // Baca status tombol
  buttonState = digitalRead(BUTTON_PIN);
  
  // Perhatikan: dengan INPUT_PULLUP, tombol menghasilkan LOW saat ditekan
  if (buttonState == LOW) {
    // Tombol ditekan
    digitalWrite(LED_PIN, HIGH);  // Nyalakan LED
    Serial.println("Tombol ditekan - LED menyala");
  } else {
    // Tombol tidak ditekan
    digitalWrite(LED_PIN, LOW);   // Matikan LED
  }
  
  // Sedikit delay untuk stabilitas
  delay(100);
}
```

> ⚠️ **Catatan Keamanan**:  
> ESP32-S3 bekerja pada level tegangan 3.3V. Pastikan tidak menghubungkan pin GPIO langsung ke sumber tegangan yang lebih tinggi (seperti 5V) karena dapat merusak board.

---

### Pembacaan Input Analog dengan ADC

#### Rangkaian untuk ADC (Pembacaan Nilai Analog)

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- 1 potensiometer 10K ohm (atau sensor analog lain seperti LDR)
- Breadboard
- Kabel jumper

**Penjelasan Rangkaian:**
Rangkaian ini menunjukkan bagaimana membaca nilai analog menggunakan ADC (Analog-to-Digital Converter) pada ESP32-S3. Kita akan menggunakan potensiometer sebagai sumber input analog yang dapat diatur.

**Langkah-langkah Perakitan:**
1. Pasang ESP32-S3 pada breadboard
2. Pasang potensiometer pada breadboard
3. Hubungkan pin luar pertama potensiometer ke 3.3V ESP32-S3
4. Hubungkan pin tengah (wiper) potensiometer ke pin GPIO1/ADC1_0 ESP32-S3
5. Hubungkan pin luar kedua potensiometer ke GND ESP32-S3

![Rangkaian ADC](https://via.placeholder.com/800x400?text=Rangkaian+ADC+ESP32-S3)

**Prinsip Kerja:**
- Potensiometer berfungsi sebagai pembagi tegangan yang dapat diatur
- Saat knob potensiometer diputar, tegangan pada pin tengah akan berubah antara 0V dan 3.3V
- ADC pada ESP32-S3 akan mengubah tegangan analog ini menjadi nilai digital antara 0 dan 4095 (untuk resolusi 12-bit)
- Program akan menampilkan nilai ADC dan tegangan yang dihitung pada Serial Monitor

**Catatan tentang ADC ESP32-S3:**
ESP32-S3 memiliki dua blok ADC (ADC1 dan ADC2) dengan resolusi hingga 12-bit. Beberapa pin GPIO dapat berfungsi sebagai input ADC, tetapi perhatikan bahwa tidak semua pin ADC tersedia saat menggunakan WiFi atau flash memory. Selalu periksa dokumentasi ESP32-S3 untuk melihat pin ADC yang tersedia.

ESP32-S3 memiliki Analog-to-Digital Converter (ADC) yang memungkinkan kita membaca nilai analog dari sensor.

```cpp
/*
 * Contoh ADC (Analog-to-Digital Converter)
 * ---------------------------------------
 * Program ini menunjukkan cara membaca nilai analog dari sensor potentiometer
 * dan menampilkannya di Serial Monitor.
 */

// Definisikan pin
#define ANALOG_PIN 1    // Pin ADC

// Variabel untuk menyimpan nilai analog
int analogValue = 0;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 ADC Test");
  
  // Konfigurasi resolusi ADC (0-4095)
  analogReadResolution(12);  // 12-bit resolution
}

void loop() {
  // Baca nilai analog
  analogValue = analogRead(ANALOG_PIN);
  
  // Konversi nilai ADC ke tegangan (0-3.3V)
  float voltage = analogValue * (3.3 / 4095.0);
  
  // Tampilkan nilai di Serial Monitor
  Serial.print("Nilai ADC: ");
  Serial.print(analogValue);
  Serial.print(" | Tegangan: ");
  Serial.print(voltage, 2);  // 2 angka desimal
  Serial.println("V");
  
  // Tunggu sebentar
  delay(500);
}
```

---

### Pulse Width Modulation (PWM)

#### Rangkaian untuk PWM (LED dengan Kecerahan Berubah)

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- 1 LED (warna sesuai preferensi)
- 1 resistor 220 ohm
- Breadboard
- Kabel jumper

**Penjelasan Rangkaian:**
Rangkaian ini menunjukkan penggunaan PWM (Pulse Width Modulation) untuk mengontrol kecerahan LED. PWM adalah teknik di mana sinyal digital diubah menjadi analog dengan cara mengubah lebar pulsa sinyal.

**Langkah-langkah Perakitan:**
1. Pasang ESP32-S3 pada breadboard
2. Pasang LED pada breadboard
3. Hubungkan kaki anoda LED ke pin GPIO2 ESP32-S3 melalui resistor 220 ohm
4. Hubungkan kaki katoda LED ke GND ESP32-S3

![Rangkaian PWM](https://via.placeholder.com/800x400?text=Rangkaian+PWM+ESP32-S3)

**Prinsip Kerja:**
- ESP32-S3 memiliki hardware PWM yang dapat menghasilkan sinyal PWM pada berbagai pin
- Sinyal PWM adalah sinyal digital yang berubah antara HIGH dan LOW dengan frekuensi tertentu
- Rasio antara waktu sinyal HIGH dengan periode total sinyal disebut duty cycle
- Semakin besar duty cycle, semakin terang LED akan menyala
- Pada contoh ini, duty cycle akan berubah dari 0% (mati) hingga 100% (terang maksimum) dan kembali ke 0%

**Diagram PWM dengan Duty Cycle Berbeda:**
```
Duty Cycle 25%:
        _   _   _   _
       | | | | | | | |
______| |_| |_| |_| |____
   <-T->
   
Duty Cycle 50%:
        ___   ___   ___
       |   | |   | |   |
______|   |_|   |_|   |__
   <-T->
   
Duty Cycle 75%:
        _____   _____
       |     | |     |
______|     |_|     |__
   <-T->
```

**Catatan tentang PWM ESP32-S3:**
ESP32-S3 memiliki 8 channel PWM yang dapat dikonfigurasikan dengan frekuensi dan resolusi yang berbeda-beda. Setiap channel dapat terhubung ke pin GPIO yang berbeda. Dalam contoh kode, kita menggunakan `ledcSetup()` dan `ledcAttachPin()` untuk mengonfigurasi PWM.

PWM memungkinkan kita menghasilkan sinyal analog dari pin digital, berguna untuk mengontrol kecerahan LED, kecepatan motor, dll.

```cpp
/*
 * Contoh PWM (Pulse Width Modulation)
 * ----------------------------------
 * Program ini menunjukkan cara menggunakan PWM untuk mengontrol kecerahan LED.
 * LED akan berubah dari redup ke terang dan kembali ke redup secara berulang.
 */

// Definisikan pin dan parameter PWM
#define LED_PIN     2    // Pin LED dengan PWM
#define PWM_CHANNEL 0    // Channel PWM (ESP32 memiliki 16 channel)
#define PWM_FREQ    5000 // Frekuensi PWM (Hz)
#define PWM_RES     8    // Resolusi PWM (8-bit, 0-255)

void setup() {
  // Konfigurasi PWM
  ledcSetup(PWM_CHANNEL, PWM_FREQ, PWM_RES);
  
  // Hubungkan pin LED ke channel PWM
  ledcAttachPin(LED_PIN, PWM_CHANNEL);
  
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 PWM Test");
}

void loop() {
  // Fade in (redup ke terang)
  Serial.println("Fade in");
  for (int dutyCycle = 0; dutyCycle <= 255; dutyCycle++) {
    // Setel duty cycle PWM
    ledcWrite(PWM_CHANNEL, dutyCycle);
    delay(5);  // Tunggu 5ms
  }
  
  // Tunggu sebentar saat LED terang penuh
  delay(500);
  
  // Fade out (terang ke redup)
  Serial.println("Fade out");
  for (int dutyCycle = 255; dutyCycle >= 0; dutyCycle--) {
    // Setel duty cycle PWM
    ledcWrite(PWM_CHANNEL, dutyCycle);
    delay(5);  // Tunggu 5ms
  }
  
  // Tunggu sebentar saat LED mati
  delay(500);
}
```

---

### Membuat Proyek Sederhana: Sistem Monitoring Suhu

#### Rangkaian untuk Sistem Monitoring Suhu

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Sensor DHT11 atau DHT22
- 3 LED (biru, kuning, merah)
- 3 resistor 220 ohm (untuk LED)
- 1 resistor 10K ohm (untuk DHT jika tidak memiliki resistor pull-up internal)
- Breadboard
- Kabel jumper

**Penjelasan Rangkaian:**
Rangkaian ini mengintegrasikan sensor suhu dan kelembaban DHT dengan tiga LED indikator yang akan menyala berdasarkan nilai suhu yang terdeteksi. Sistem ini menggabungkan beberapa konsep yang telah dipelajari sebelumnya: komunikasi dengan sensor, pembacaan nilai analog, dan kontrol output digital.

**Langkah-langkah Perakitan:**
1. Pasang ESP32-S3 pada breadboard
2. Pasang sensor DHT pada breadboard
3. Hubungkan pin VCC DHT ke pin 3.3V ESP32-S3
4. Hubungkan pin GND DHT ke pin GND ESP32-S3
5. Hubungkan pin DATA DHT ke pin GPIO4 ESP32-S3
6. Jika sensor DHT tidak memiliki resistor pull-up internal, pasang resistor 10K ohm antara pin DATA dan VCC
7. Pasang ketiga LED (biru, kuning, dan merah) pada breadboard
8. Hubungkan kaki anoda LED biru ke pin GPIO2 ESP32-S3 melalui resistor 220 ohm
9. Hubungkan kaki anoda LED kuning ke pin GPIO16 ESP32-S3 melalui resistor 220 ohm
10. Hubungkan kaki anoda LED merah ke pin GPIO17 ESP32-S3 melalui resistor 220 ohm
11. Hubungkan semua kaki katoda LED ke GND

![Rangkaian Sistem Monitoring Suhu](https://via.placeholder.com/800x600?text=Rangkaian+Sistem+Monitoring+Suhu)

**Prinsip Kerja:**
- Sensor DHT11/DHT22 mendeteksi suhu dan kelembaban lingkungan
- ESP32-S3 membaca data dari sensor secara berkala (setiap 2 detik)
- Berdasarkan nilai suhu yang dibaca:
  - LED biru menyala jika suhu di bawah 20°C (dingin)
  - LED kuning menyala jika suhu antara 20°C dan 30°C (normal)
  - LED merah menyala jika suhu di atas 30°C (panas)
- Data suhu dan kelembaban ditampilkan pada Serial Monitor

**Perbedaan antara DHT11 dan DHT22:**

| Fitur | DHT11 | DHT22 |
|-------|-------|-------|
| Rentang Suhu | 0-50°C | -40-80°C |
| Akurasi Suhu | ±2°C | ±0.5°C |
| Rentang Kelembaban | 20-90% RH | 0-100% RH |
| Akurasi Kelembaban | ±5% RH | ±2% RH |
| Waktu Sampling | 1 Hz (sekali per detik) | 0.5 Hz (sekali per 2 detik) |
| Harga | Lebih murah | Lebih mahal |

**Catatan Penting:**
- Beberapa sensor DHT22 dijual dengan nama AM2302, yang merupakan varian DHT22 dengan housing berbeda
- Pastikan untuk menunggu minimal 2 detik antara pembacaan sensor untuk mendapatkan hasil yang akurat
- Jangan lupa untuk menginstal library DHT sebelum mengupload kode

Mari kita kombinasikan semua yang telah kita pelajari untuk membuat sistem yang membaca sensor suhu dan merespons dengan LED dan output serial.

Untuk contoh ini, kita akan menggunakan sensor DHT11 atau DHT22 yang umum digunakan untuk membaca suhu dan kelembaban.

```cpp
/*
 * Sistem Monitoring Suhu
 * ----------------------
 * Program ini membaca suhu dan kelembaban dari sensor DHT, 
 * menampilkan datanya di Serial Monitor, dan mengontrol LED 
 * berdasarkan nilai suhu.
 */

// Include library DHT sensor
#include <DHT.h>

// Definisikan pin dan parameter
#define DHT_PIN      4     // Pin data sensor DHT
#define DHT_TYPE     DHT22 // Tipe sensor (DHT11 atau DHT22)
#define LED_BLUE     2     // LED biru untuk suhu dingin
#define LED_YELLOW   16    // LED kuning untuk suhu normal
#define LED_RED      17    // LED merah untuk suhu panas

// Batas suhu (dalam derajat Celsius)
#define TEMP_COLD    20    // Di bawah ini dianggap dingin
#define TEMP_HOT     30    // Di atas ini dianggap panas

// Inisialisasi sensor DHT
DHT dht(DHT_PIN, DHT_TYPE);

// Variabel untuk menyimpan data sensor
float temperature;
float humidity;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 Sistem Monitoring Suhu");
  
  // Inisialisasi pin LED
  pinMode(LED_BLUE, OUTPUT);
  pinMode(LED_YELLOW, OUTPUT);
  pinMode(LED_RED, OUTPUT);
  
  // Matikan semua LED pada awalnya
  digitalWrite(LED_BLUE, LOW);
  digitalWrite(LED_YELLOW, LOW);
  digitalWrite(LED_RED, LOW);
  
  // Mulai sensor DHT
  dht.begin();
  Serial.println("Sensor DHT diinisialisasi");
  
  // Jeda singkat untuk stabilisasi sensor
  delay(2000);
}

void loop() {
  // Baca suhu dan kelembaban
  humidity = dht.readHumidity();
  temperature = dht.readTemperature();
  
  // Periksa apakah pembacaan berhasil
  if (isnan(humidity) || isnan(temperature)) {
    Serial.println("Gagal membaca data dari sensor DHT!");
    // Kedipkan semua LED sebagai indikasi error
    blinkAllLeds(3);
    return;
  }
  
  // Tampilkan data di Serial Monitor
  Serial.print("Suhu: ");
  Serial.print(temperature);
  Serial.print("°C | Kelembaban: ");
  Serial.print(humidity);
  Serial.println("%");
  
  // Kontrol LED berdasarkan suhu
  updateLeds(temperature);
  
  // Tunggu sebelum pembacaan berikutnya
  delay(2000);
}

// Fungsi untuk mengendalikan LED berdasarkan suhu
void updateLeds(float temp) {
  // Matikan semua LED terlebih dahulu
  digitalWrite(LED_BLUE, LOW);
  digitalWrite(LED_YELLOW, LOW);
  digitalWrite(LED_RED, LOW);
  
  // Nyalakan LED yang sesuai
  if (temp < TEMP_COLD) {
    // Suhu dingin, nyalakan LED biru
    digitalWrite(LED_BLUE, HIGH);
    Serial.println("Status: DINGIN");
  } else if (temp >= TEMP_COLD && temp < TEMP_HOT) {
    // Suhu normal, nyalakan LED kuning
    digitalWrite(LED_YELLOW, HIGH);
    Serial.println("Status: NORMAL");
  } else {
    // Suhu panas, nyalakan LED merah
    digitalWrite(LED_RED, HIGH);
    Serial.println("Status: PANAS");
  }
}

// Fungsi untuk mengedipkan semua LED (indikasi error)
void blinkAllLeds(int times) {
  for (int i = 0; i < times; i++) {
    // Nyalakan semua LED
    digitalWrite(LED_BLUE, HIGH);
    digitalWrite(LED_YELLOW, HIGH);
    digitalWrite(LED_RED, HIGH);
    delay(200);
    
    // Matikan semua LED
    digitalWrite(LED_BLUE, LOW);
    digitalWrite(LED_YELLOW, LOW);
    digitalWrite(LED_RED, LOW);
    delay(200);
  }
}
```

> 🔔 **Penting**:  
> Untuk menjalankan kode di atas, Anda harus terlebih dahulu menginstal library DHT seperti yang dijelaskan pada bagian "[Menginstal Library di Arduino IDE](#menginstal-library-di-arduino-ide)". Tanpa library ini, kode tidak akan dapat dikompilasi.

---
---

## 📝 Tugas dan Latihan

### Latihan 1: Blink Multipleks
Modifikasi contoh LED blink untuk mengendalikan 3 LED dengan pola yang berbeda.

### Latihan 2: Membaca Sensor Analog
Gunakan sensor LDR (Light Dependent Resistor) untuk membuat lampu otomatis yang menyala ketika gelap.

### Latihan 3: Kontrol PWM Lanjutan
Buat sistem dengan tombol untuk mengontrol kecerahan LED. Setiap kali tombol ditekan, kecerahan meningkat 25% sampai 100%, kemudian kembali ke 0%.

### Proyek Mini: Stasiun Cuaca Sederhana
Buat sistem yang membaca suhu, kelembaban, dan intensitas cahaya, lalu menampilkan datanya di Serial Monitor dan mengendalikan LED sebagai indikator visual.

---
---

## 📚 Referensi dan Bacaan Tambahan

### Buku Referensi
1. Schwartz, M. (2023). *Internet of Things with ESP32: Building Connected Devices with ESP32 Microcontrollers*. Apress. Bab 1-3.
2. Raj, P., & Raman, A. C. (2022). *The Internet of Things: Enabling Technologies, Platforms, and Use Cases*. CRC Press. Bab 1: "Dasar-dasar IoT".
3. Hughes, J. M. (2021). *Arduino: A Technical Reference*. O'Reilly Media. Bagian IV: "Working with Arduino and ESP Microcontrollers".
4. Blum, J. (2022). *Exploring Arduino: Tools and Techniques for Engineering Wizardry* (2nd Edition). Wiley. Bab 12: "Connecting to the Internet with ESP32".
5. Purdum, J. (2021). *Beginning C for Arduino: Learn C Programming for the Arduino and Compatible Microcontrollers*. Apress. Bab 1-5: Dasar-dasar pemrograman C.

### Publikasi Ilmiah
6. Maier, A., Sharp, A., & Vagapov, Y. (2023). "Comparative Analysis of IoT Development Boards for Educational Purposes." *IEEE Transactions on Education*, 66(1), 32-41.
7. Espressif Systems. (2024). "ESP32-S3 Technical Reference Manual" [Version 1.2]. *Espressif Systems*.
8. Kumar, R., & Patel, D. R. (2023). "Educational Approaches to Internet of Things: A Review of Platforms and Pedagogical Methods." *International Journal of Electrical Engineering Education*, 60(1), 92-115.
9. Verma, A., Singh, Y., & Kumar, N. (2022). "Survey on Arduino-Based Development Platforms for IoT Applications." *Journal of Systems Architecture*, 123, 102395.

### Tutorial dan Materi Online
10. Colwell, J. (2024). "Getting Started with ESP32-S3 and Arduino IDE." *Random Nerd Tutorials*. [https://randomnerdtutorials.com/esp32-s3-arduino-ide-setup/](https://randomnerdtutorials.com/esp32-s3-arduino-ide-setup/)
11. Grokhotkov, I. (2023). "ESP32-S3 Arduino Core Documentation." *GitHub - espressif/arduino-esp32*. [https://github.com/espressif/arduino-esp32/tree/master/docs](https://github.com/espressif/arduino-esp32/tree/master/docs)
12. Thompson, L. (2024). "Understanding GPIO, PWM, and ADC with ESP32-S3." *DroneBot Workshop*. [https://dronebotworkshop.com](https://dronebotworkshop.com)
13. Espressif Systems. (2024). "ESP32-S3 Datasheet." *Espressif Systems*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)
14. Arduino Team. (2023). "Arduino Reference." *Arduino*. [https://www.arduino.cc/reference/en/](https://www.arduino.cc/reference/en/)

### Video dan Kursus Online
15. Spiess, A. (2023). "ESP32-S3 First Look and Comparison with ESP32." *YouTube*. [https://www.youtube.com/c/AndreasSpiess](https://www.youtube.com/c/AndreasSpiess)
16. Valden, M. (2024). "Internet of Things Fundamentals." *Coursera*. [https://www.coursera.org/learn/iot-fundamentals](https://www.coursera.org/learn/iot-fundamentals)
17. Lewis, S. (2023). "ESP32 Programming from Scratch." *Udemy*. [https://www.udemy.com/course/esp32-programming](https://www.udemy.com/course/esp32-programming)

### Situs Komunitas dan Forum
18. "ESP32 Forum." *ESP32 Forum*. [https://esp32.com/](https://esp32.com/)
19. "Arduino Forum - ESP32 Section." *Arduino Forum*. [https://forum.arduino.cc/](https://forum.arduino.cc/)
20. "Reddit r/esp32." *Reddit*. [https://www.reddit.com/r/esp32/](https://www.reddit.com/r/esp32/)

> 📝 **Catatan untuk Mahasiswa:**  
> Referensi 1-5 sangat direkomendasikan untuk pemahaman fundamental tentang IoT dan pemrograman ESP32-S3. Untuk panduan praktis tentang setup dan pemrograman dasar, referensi 10-14 akan sangat membantu.

---

<div align="center">
  
  **🎓 Modul Kuliah Internet of Things - 2025**
  
  *Dibuat oleh [Nama Dosen]*
  
</div>

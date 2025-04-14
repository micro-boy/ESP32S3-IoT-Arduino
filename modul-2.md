# 📚 Modul 2: Sensor dan Aktuator untuk Aplikasi IoT 

<div align="center">
  <img src="https://github.com/user-attachments/assets/c623f4aa-d366-4ab2-a380-9abb9fc2d59e" width="400">
  
  *Kuliah Internet of Things dengan ESP32-S3 dan Arduino IDE*
  
</div>
<div align="center">
  <img src="https://img.shields.io/badge/Tingkat-Pemula-brightgreen" alt="Tingkat: Pemula">
  <img src="https://img.shields.io/badge/Platform-ESP32--S3-blue" alt="Platform: ESP32-S3">
  <img src="https://img.shields.io/badge/IDE-Arduino-red" alt="IDE: Arduino">
</div>

---

## 📋 Daftar Isi

- [Pertemuan 1: Pengantar Sensor dalam IoT](#-pertemuan-1-pengantar-sensor-dalam-iot)
- [Pertemuan 2: Sensor Lingkungan](#-pertemuan-2-sensor-lingkungan)
  - [BME680](#bme680-sensor-suhu-kelembaban-tekanan-dan-gas)
  - [LDR Digital](#ldr-digital-light-dependent-resistor-dengan-output-digital)
  - [BH1750](#bh1750-sensor-cahaya-digital-dengan-pengukuran-lux)
- [Pertemuan 3: Sensor Gerak dan Posisi](#-pertemuan-3-sensor-gerak-dan-posisi)
  - [HC-SR04](#1-hc-sr04-sensor-ultrasonik)
  - [PIR](#2-pir-passive-infrared-motion-sensor)
  - [MPU6050](#3-mpu6050-akselerometer-dan-giroskop-6-axis)
  - [ESP32-S3 Touch Sensor](#4-esp32-s3-touch-sensor-kapasitif)
- [Pertemuan 4: Integrasi Multi-Sensor](#-pertemuan-4-integrasi-multi-sensor)
- [Tugas dan Latihan](#-tugas-dan-latihan)
- [Referensi dan Bacaan Tambahan](#-referensi-dan-bacaan-tambahan)

---

## 💡 Pertemuan 1: Pengantar Sensor dalam IoT

### Apa itu Sensor?

Sensor adalah perangkat yang dapat mendeteksi perubahan dalam lingkungan fisik dan mengubahnya menjadi sinyal yang dapat diukur. Dalam konteks IoT, sensor berfungsi sebagai "indra" dari sistem, mengumpulkan informasi dari dunia nyata agar dapat diproses oleh mikrokontroler.

> 💬 **Definisi Sederhana:**  
> Sensor adalah perangkat yang mengubah besaran fisik (seperti suhu, cahaya, tekanan) menjadi sinyal listrik yang dapat dibaca dan diproses oleh mikrokontroler.

### Klasifikasi Sensor

Dalam kursus ini, kita akan fokus pada dua kategori utama sensor:

#### 1. Sensor Lingkungan
- **BME680**: Mengukur suhu, kelembaban, tekanan, dan kualitas udara (gas VOC)
- **LDR Digital**: Mendeteksi perubahan intensitas cahaya dengan output digital
- **BH1750**: Mengukur intensitas cahaya dengan presisi tinggi (dalam lux)

#### 2. Sensor Gerak dan Posisi
- **HC-SR04**: Mengukur jarak menggunakan gelombang ultrasonik
- **PIR**: Mendeteksi gerakan berdasarkan perubahan radiasi inframerah
- **MPU6050**: Mendeteksi orientasi, percepatan, dan gerakan rotasi
- **ESP32-S3 Touch Sensor**: Mendeteksi sentuhan kapasitif

### Protokol Komunikasi untuk Sensor

Sensor yang kita gunakan berkomunikasi dengan ESP32-S3 melalui beberapa protokol:

#### 1. Digital I/O
Membaca status HIGH/LOW melalui pin GPIO (LDR Digital, PIR, ESP32-S3 Touch Sensor).

#### 2. I2C (Inter-Integrated Circuit)
Protokol komunikasi serial yang menggunakan dua jalur: SCL (Serial Clock) dan SDA (Serial Data).

**Sensor dalam kursus ini yang menggunakan I2C:**
- BME680 (alamat 0x76 atau 0x77)
- BH1750 (alamat 0x23 atau 0x5C)
- MPU6050 (alamat 0x68 atau 0x69)

**Pin I2C default pada ESP32-S3:**
- SCL: GPIO22
- SDA: GPIO21

#### 3. Time-Based (HC-SR04)
Mengukur waktu perjalanan pulsa (pulseIn) untuk menghitung jarak.

### I2C Scanner - Alat Penting untuk Debugging

Tool I2C Scanner sangat berguna untuk memastikan sensor I2C Anda berfungsi dan terhubung dengan benar:

```cpp
/*
 * I2C Scanner
 * ----------
 * Tool sederhana untuk mendeteksi alamat perangkat I2C yang terhubung.
 * Sangat berguna ketika menggunakan banyak sensor I2C.
 */

#include <Wire.h>

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  while (!Serial) delay(10);
  
  Serial.println("ESP32-S3 I2C Scanner");
  
  // Inisialisasi I2C bus
  Wire.begin();
}

void loop() {
  byte error, address;
  int deviceCount = 0;
  
  Serial.println("Scanning...");
  
  // Scan alamat dari 1 sampai 127
  for(address = 1; address < 127; address++) {
    Wire.beginTransmission(address);
    error = Wire.endTransmission();
    
    if (error == 0) {
      Serial.print("Perangkat I2C ditemukan pada alamat 0x");
      if (address < 16) Serial.print("0");
      Serial.println(address, HEX);
      deviceCount++;
      
      // Identifikasi sensor berdasarkan alamat
      if (address == 0x76 || address == 0x77) {
        Serial.println("  Kemungkinan BME680/BME280");
      } else if (address == 0x23 || address == 0x5C) {
        Serial.println("  Kemungkinan BH1750");
      } else if (address == 0x68 || address == 0x69) {
        Serial.println("  Kemungkinan MPU6050");
      }
    }
  }
  
  if (deviceCount == 0) {
    Serial.println("Tidak ada perangkat I2C ditemukan\n");
  } else {
    Serial.print("Ditemukan ");
    Serial.print(deviceCount);
    Serial.println(" perangkat\n");
  }
  
  delay(5000);  // Scan ulang setiap 5 detik
}
```

### Teknik Dasar Filtering Pembacaan Sensor

Sensor sering menghasilkan data dengan noise. Berikut teknik filtering sederhana yang akan kita gunakan:

#### Rata-rata Bergerak (Moving Average)

```cpp
// Implementasi sederhana filter moving average
#define WINDOW_SIZE 10
float readings[WINDOW_SIZE];  // Array untuk penyimpanan pembacaan
int readIndex = 0;           // Indeks pembacaan saat ini
float total = 0;             // Total pembacaan
float average = 0;           // Rata-rata pembacaan

void setup() {
  // Inisialisasi array dengan nol
  for (int i = 0; i < WINDOW_SIZE; i++) {
    readings[i] = 0;
  }
}

float getFilteredReading(float newReading) {
  // Kurangi nilai tertua dari total
  total = total - readings[readIndex];
  // Tambahkan pembacaan baru ke array pada posisi indeks saat ini
  readings[readIndex] = newReading;
  // Tambahkan pembacaan baru ke total
  total = total + readings[readIndex];
  // Maju ke posisi berikutnya di array
  readIndex = (readIndex + 1) % WINDOW_SIZE;
  // Hitung rata-rata
  average = total / WINDOW_SIZE;
  
  return average;
}
```

---

## 🌡️ Pertemuan 2: Sensor Lingkungan

Sensor lingkungan memungkinkan perangkat IoT mengukur dan memantau berbagai parameter lingkungan seperti suhu, kelembaban, tekanan, kualitas udara, dan cahaya.

### BME680 (Sensor Suhu, Kelembaban, Tekanan, dan Gas)

BME680 adalah sensor lingkungan canggih produksi Bosch yang menggabungkan empat fungsi pengukuran dalam satu chip: suhu, kelembaban, tekanan barometrik, dan gas (VOC - Volatile Organic Compounds).

<div align="center">
   <img src="https://github.com/user-attachments/assets/9962b7b7-baef-42c9-99af-6dfd36e8e76c" width="300">
  <p><em>Sensor BME680</em></p>
</div>

**Fitur BME680:**
- Rentang suhu: -40°C hingga +85°C (akurasi ±1°C)
- Rentang kelembaban: 0-100% RH (akurasi ±3%)
- Rentang tekanan: 300-1100 hPa (akurasi ±1 hPa)
- Sensor gas untuk mendeteksi VOC dan kualitas udara
- Antarmuka I2C (alamat 0x76 atau 0x77) dan SPI
- Konsumsi daya rendah: 3.7 μA pada 1 Hz pengukuran (hanya suhu dan tekanan)

**Rangkaian dengan BME680 (I2C):**

<div align="center">
   <img src="https://github.com/user-attachments/assets/44a1b1ee-a96a-4c6c-8fee-84776aaede20" width="300">
  <p><em>Rangkaian BME680 dengan ESP32-S3 (I2C)</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Modul sensor BME680
- Kabel jumper

**Koneksi I2C:**
- VCC BME680 → 3.3V ESP32-S3
- GND BME680 → GND ESP32-S3
- SCL BME680 → GPIO22 ESP32-S3 (default I2C SCL)
- SDA BME680 → GPIO21 ESP32-S3 (default I2C SDA)

**Contoh Kode BME680:**

```cpp
/*
 * ESP32-S3 dengan Sensor BME680
 * ---------------------------
 * Program ini membaca suhu, kelembaban, tekanan, dan kualitas udara
 * dari sensor BME680 dan menampilkan hasilnya melalui Serial Monitor.
 */

#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME680.h>

// Definisikan sea level pressure di lokasi Anda untuk perhitungan ketinggian
#define SEALEVELPRESSURE_HPA (1013.25)

// Inisialisasi objek BME680
Adafruit_BME680 bme;

// Variabel untuk filter
#define READINGS_COUNT 5
float tempReadings[READINGS_COUNT];
float humReadings[READINGS_COUNT];
float presReadings[READINGS_COUNT];
float gasReadings[READINGS_COUNT];
int readIndex = 0;
float tempTotal = 0, humTotal = 0, presTotal = 0, gasTotal = 0;
float avgTemp = 0, avgHum = 0, avgPres = 0, avgGas = 0;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 BME680 Test");
  
  // Inisialisasi I2C
  Wire.begin();
  
  // Coba inisialisasi sensor pada alamat 0x76
  if (!bme.begin(0x76)) {
    // Jika gagal, coba alamat 0x77
    if (!bme.begin(0x77)) {
      Serial.println("Tidak dapat menemukan sensor BME680, cek wiring atau alamat!");
      while (1);  // Loop terus jika sensor tidak ditemukan
    }
  }
  
  Serial.println("BME680 berhasil diinisialisasi!");
  
  // Konfigurasi pengaturan sensor
  bme.setTemperatureOversampling(BME680_OS_8X);
  bme.setHumidityOversampling(BME680_OS_2X);
  bme.setPressureOversampling(BME680_OS_4X);
  bme.setIIRFilterSize(BME680_FILTER_SIZE_3);
  bme.setGasHeater(320, 150); // 320°C selama 150 ms
  
  // Inisialisasi array pembacaan dengan 0
  for (int i = 0; i < READINGS_COUNT; i++) {
    tempReadings[i] = 0;
    humReadings[i] = 0;
    presReadings[i] = 0;
    gasReadings[i] = 0;
  }
}

void loop() {
  // Mulai pengukuran gas VOC
  if (!bme.performReading()) {
    Serial.println("Gagal melakukan pembacaan!");
    return;
  }
  
  // Baca data sensor
  float temperature = bme.temperature;
  float humidity = bme.humidity;
  float pressure = bme.pressure / 100.0F;  // Convert Pa to hPa
  float altitude = bme.readAltitude(SEALEVELPRESSURE_HPA);
  float gas_resistance = bme.gas_resistance / 1000.0F;  // kOhm
  
  // Update filter suhu
  tempTotal = tempTotal - tempReadings[readIndex];
  tempReadings[readIndex] = temperature;
  tempTotal = tempTotal + tempReadings[readIndex];
  avgTemp = tempTotal / READINGS_COUNT;
  
  // Update filter kelembaban
  humTotal = humTotal - humReadings[readIndex];
  humReadings[readIndex] = humidity;
  humTotal = humTotal + humReadings[readIndex];
  avgHum = humTotal / READINGS_COUNT;
  
  // Update filter tekanan
  presTotal = presTotal - presReadings[readIndex];
  presReadings[readIndex] = pressure;
  presTotal = presTotal + presReadings[readIndex];
  avgPres = presTotal / READINGS_COUNT;
  
  // Update filter gas
  gasTotal = gasTotal - gasReadings[readIndex];
  gasReadings[readIndex] = gas_resistance;
  gasTotal = gasTotal + gasReadings[readIndex];
  avgGas = gasTotal / READINGS_COUNT;
  
  // Update index
  readIndex = (readIndex + 1) % READINGS_COUNT;
  
  // Tampilkan hasil
  Serial.println("---------------------");
  Serial.print("Suhu: ");
  Serial.print(avgTemp, 1);
  Serial.println(" °C");
  
  Serial.print("Kelembaban: ");
  Serial.print(avgHum, 1);
  Serial.println(" %");
  
  Serial.print("Tekanan: ");
  Serial.print(avgPres, 1);
  Serial.println(" hPa");
  
  Serial.print("Ketinggian: ~");
  Serial.print(altitude, 0);
  Serial.println(" m (berdasarkan tekanan laut standar)");
  
  Serial.print("Resistansi Gas: ");
  Serial.print(avgGas, 1);
  Serial.println(" kOhm");
  
  // Perkiraan kualitas udara sederhana berdasarkan resistansi gas
  // Nilai resistansi lebih tinggi = udara lebih bersih
  Serial.print("Kualitas Udara: ");
  if (avgGas > 50) {
    Serial.println("Sangat Baik");
  } else if (avgGas > 20) {
    Serial.println("Baik");
  } else if (avgGas > 10) {
    Serial.println("Sedang");
  } else if (avgGas > 5) {
    Serial.println("Buruk");
  } else {
    Serial.println("Sangat Buruk");
  }
  
  Serial.println();
  delay(2000);
}
```

> ⚠️ **Catatan Penting:**  
> Untuk menggunakan BME680, Anda perlu menginstal library Adafruit BME680 dan Adafruit Sensor. Keduanya dapat diinstal melalui Library Manager Arduino IDE. Cari "Adafruit BME680" dan instal bersama dengan dependensinya.

**Aplikasi BME680:**
1. Pemantauan kualitas udara dalam ruangan
2. Stasiun cuaca IoT
3. Smart home dan sistem HVAC (Heating, Ventilation, and Air Conditioning)
4. Pemantauan kondisi lingkungan untuk pertanian presisi

### LDR Digital (Light Dependent Resistor dengan Output Digital)

LDR (Light Dependent Resistor) digital adalah versi modul yang mengintegrasikan LDR tradisional dengan komparator atau ADC untuk memberikan output digital. Ini memudahkan interfacing dengan mikrokontroler.

<div align="center">
   <img src="https://github.com/user-attachments/assets/f537585a-8107-4cd3-9e48-660a3b7eff08" width="300">
  <p><em>Modul LDR Digital</em></p>
</div>

**Fitur Modul LDR Digital:**
- Output digital (HIGH/LOW) berdasarkan ambang cahaya
- Potensiometer untuk mengatur sensitivitas/threshold
- Indikator LED pada modul
- Mudah digunakan tanpa perlu ADC

**Rangkaian dengan LDR Digital:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/3d67729a-ddf4-4d25-8e5c-111e7877dd32" width="300">
  <p><em>Rangkaian LDR Digital dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Modul LDR Digital
- Kabel jumper

**Koneksi:**
- VCC LDR module → 3.3V atau 5V ESP32-S3 (sesuai dengan modul)
- GND LDR module → GND ESP32-S3
- DO (Digital Output) LDR module → GPIO5 ESP32-S3
- AO (Analog Output) LDR module → Tidak digunakan dalam contoh ini (opsional)

**Contoh Kode LDR Digital:**

```cpp
/*
 * ESP32-S3 dengan Sensor LDR Digital
 * ------------------------------
 * Program ini membaca output digital dari modul LDR
 * untuk mendeteksi kondisi terang/gelap.
 */

// Definisikan pin
#define LDR_DIGITAL_PIN 5
#define LED_PIN 2  // LED bawaan ESP32-S3

// Variabel untuk debounce dan status
bool currentLightState = false;  // false = gelap, true = terang
bool lastLightState = false;
unsigned long lastDebounceTime = 0;
unsigned long debounceDelay = 50;  // Waktu debounce dalam ms

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 LDR Digital Test");
  
  // Konfigurasi pin
  pinMode(LDR_DIGITAL_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  
  // Baca status awal
  int initialReading = digitalRead(LDR_DIGITAL_PIN);
  currentLightState = (initialReading == LOW);  // Biasanya LOW saat cahaya terdeteksi
  lastLightState = currentLightState;
  
  // Set LED sesuai status awal (ON saat gelap)
  digitalWrite(LED_PIN, !currentLightState);
  
  // Tampilkan status awal
  Serial.print("Status awal: ");
  Serial.println(currentLightState ? "TERANG" : "GELAP");
}

void loop() {
  // Baca status sensor LDR
  int ldrReading = digitalRead(LDR_DIGITAL_PIN);
  bool reading = (ldrReading == LOW);  // Konversi ke boolean (true = terang, false = gelap)
  
  // Jika pembacaan berubah, reset timer debounce
  if (reading != lastLightState) {
    lastDebounceTime = millis();
  }
  
  // Periksa jika durasi debounce sudah berlalu
  if ((millis() - lastDebounceTime) > debounceDelay) {
    // Jika pembacaan stabil dan berbeda dari status saat ini
    if (reading != currentLightState) {
      currentLightState = reading;
      
      // Tampilkan status
      Serial.print("Kondisi berubah: ");
      if (currentLightState) {
        Serial.println("TERANG");
        digitalWrite(LED_PIN, LOW);  // Matikan LED saat terang
      } else {
        Serial.println("GELAP");
        digitalWrite(LED_PIN, HIGH);  // Nyalakan LED saat gelap
      }
    }
  }
  
  // Simpan pembacaan untuk perbandingan berikutnya
  lastLightState = reading;
  
  delay(10);  // Delay kecil untuk stabilitas
}
```

**Penyesuaian Sensitivitas:**
Sebagian besar modul LDR digital memiliki potensiometer yang dapat Anda putar untuk menyesuaikan level ambang batas cahaya. Putar searah jarum jam untuk meningkatkan sensitivitas (bereaksi pada level cahaya yang lebih tinggi) atau berlawanan arah jarum jam untuk mengurangi sensitivitas.

**Aplikasi LDR Digital:**
1. Kontrol pencahayaan otomatis (menyalakan lampu saat gelap)
2. Deteksi siang/malam untuk sistem keamanan
3. Penghematan energi pada perangkat battery-powered
4. Deteksi keberadaan objek dengan interupsi cahaya

### BH1750 (Sensor Cahaya Digital dengan Pengukuran Lux)

BH1750 adalah sensor cahaya digital yang memberikan pengukuran intensitas cahaya yang presisi dalam satuan lux (lx). Sensor ini ideal untuk aplikasi yang membutuhkan pengukuran cahaya yang akurat.

<div align="center">
   <img src="https://github.com/user-attachments/assets/dda5d553-88d1-4d5e-8e2c-1c7ed8efdb52" width="300">
  <p><em>Sensor BH1750</em></p>
</div>

**Fitur BH1750:**
- Rentang pengukuran: 1-65535 lux
- Antarmuka I2C (alamat 0x23 atau 0x5C)
- Respon spektral dekat dengan respon mata manusia
- Presisi dan akurasi tinggi
- Konsumsi daya rendah (~0.2mA saat aktif)
- Kemampuan mode resolusi tinggi dan rendah

**Rangkaian dengan BH1750:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/6aa8504e-0d73-459e-b165-b1956c6d47e9" width="300">
  <p><em>Rangkaian BH1750 dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Modul sensor BH1750
- Kabel jumper

**Koneksi:**
- VCC BH1750 → 3.3V ESP32-S3
- GND BH1750 → GND ESP32-S3
- SCL BH1750 → GPIO22 ESP32-S3 (default I2C SCL)
- SDA BH1750 → GPIO21 ESP32-S3 (default I2C SDA)
- ADDR BH1750 → tidak terhubung (untuk alamat default 0x23) atau ke 3.3V (untuk alamat 0x5C)

**Contoh Kode BH1750:**

```cpp
/*
 * ESP32-S3 dengan Sensor BH1750
 * ---------------------------
 * Program ini membaca intensitas cahaya dari sensor BH1750
 * dan menampilkan hasilnya dalam satuan lux.
 */

#include <Wire.h>
#include <BH1750.h>

// Inisialisasi objek BH1750
BH1750 lightMeter;

// Variabel untuk rata-rata bergerak
#define READINGS_COUNT 5
float readings[READINGS_COUNT];
int readIndex = 0;
float total = 0;
float averageLux = 0;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 BH1750 Test");
  
  // Inisialisasi I2C
  Wire.begin();
  
  // Inisialisasi BH1750
  lightMeter.begin();
  Serial.println("BH1750 Diinisialisasi");
  
  // Atur mode pengukuran (opsional)
  // lightMeter.configure(BH1750::CONTINUOUS_HIGH_RES_MODE_2);
  
  // Inisialisasi array pembacaan dengan 0
  for (int i = 0; i < READINGS_COUNT; i++) {
    readings[i] = 0;
  }
}

void loop() {
  // Baca intensitas cahaya
  float lux = lightMeter.readLightLevel();
  
  // Update rata-rata bergerak
  total = total - readings[readIndex];
  readings[readIndex] = lux;
  total = total + readings[readIndex];
  readIndex = (readIndex + 1) % READINGS_COUNT;
  averageLux = total / READINGS_COUNT;
  
  // Tampilkan hasil
  Serial.print("Intensitas Cahaya: ");
  Serial.print(lux);
  Serial.print(" lx (Raw) | ");
  Serial.print(averageLux);
  Serial.println(" lx (Avg)");
  
  // Tambahkan deskripsi kondisi cahaya
  Serial.print("Kondisi: ");
  if (averageLux < 10) {
    Serial.println("Sangat gelap (malam tanpa bulan)");
  } else if (averageLux < 50) {
    Serial.println("Gelap (malam berbulan/senja)");
  } else if (averageLux < 200) {
    Serial.println("Redup (dalam ruangan/berawan)");
  } else if (averageLux < 500) {
    Serial.println("Normal dalam ruangan");
  } else if (averageLux < 1000) {
    Serial.println("Terang (hari berawan)");
  } else if (averageLux < 10000) {
    Serial.println("Sangat terang (sinar matahari tidak langsung)");
  } else {
    Serial.println("Ekstrem terang (cahaya matahari langsung)");
  }
  
  Serial.println();
  delay(1000);
}
```

> 💡 **Tip:**  
> Sensor BH1750 memiliki beberapa mode operasi yang menawarkan keseimbangan antara akurasi, resolusi, dan waktu pengukuran:
> - `CONTINUOUS_HIGH_RES_MODE` (default): 1 lx resolusi, 120ms waktu pengukuran
> - `CONTINUOUS_HIGH_RES_MODE_2`: 0.5 lx resolusi, 120ms waktu pengukuran
> - `CONTINUOUS_LOW_RES_MODE`: 4 lx resolusi, 16ms waktu pengukuran (lebih cepat)

**Perbandingan LDR Digital dan BH1750:**

| Fitur | LDR Digital | BH1750 |
|-------|-------------|--------|
| Output | Digital (HIGH/LOW) | Digital (nilai lux) |
| Presisi | Rendah (hanya threshold) | Tinggi (1 lux resolusi) |
| Pengukuran | Relatif (terang/gelap) | Absolut (dalam lux) |
| Harga | Lebih murah | Lebih mahal |
| Antarmuka | Pin digital | I2C |
| Kalibrasi | Manual (via potensiometer) | Terkalibrasi pabrik |
| Aplikasi | Deteksi siang/malam, otomasi sederhana | Pengukuran cahaya presisi, kontrol pencahayaan |

---

## 🔄 Pertemuan 3: Sensor Gerak dan Posisi

Sensor gerak dan posisi memungkinkan perangkat IoT mendeteksi pergerakan, posisi, orientasi, dan percepatan. Sensor-sensor ini penting untuk berbagai aplikasi seperti sistem keamanan, pemantauan aktivitas, deteksi jatuh, navigasi, dan banyak lagi.

### 1. HC-SR04 (Sensor Ultrasonik)

HC-SR04 adalah sensor ultrasonik yang digunakan untuk mengukur jarak ke objek. Sensor ini bekerja dengan mengirimkan gelombang ultrasonik dan mengukur waktu yang diperlukan untuk gelombang memantul kembali.

<div align="center">
   <img src="https://github.com/user-attachments/assets/dda5d553-88d1-4d5e-8e2c-1c7ed8efdb52" width="300">
  <p><em>Sensor ultrasonik HC-SR04</em></p>
</div>

**Karakteristik HC-SR04:**
- Rentang pengukuran: 2cm hingga 400cm
- Resolusi: 0.3cm
- Sudut pengukuran: 15 derajat
- Tegangan operasi: 5V

**Prinsip Kerja HC-SR04:**
1. Mikrokontroler mengirim pulsa 10μs ke pin TRIG
2. Sensor mengirimkan 8 pulsa ultrasonik 40kHz
3. Gelombang memantul dari objek dan kembali ke sensor
4. Pin ECHO menjadi HIGH selama gelombang melakukan perjalanan
5. Waktu ECHO dalam keadaan HIGH proporsional dengan jarak

**Rangkaian dengan HC-SR04:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/6aa8504e-0d73-459e-b165-b1956c6d47e9" width="300">
  <p><em>Rangkaian HC-SR04 dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Sensor ultrasonik HC-SR04
- Resistor 1K dan 2K (untuk pembagi tegangan dari 5V ke 3.3V)
- Kabel jumper

**Koneksi:**
- VCC HC-SR04 → 5V ESP32-S3
- GND HC-SR04 → GND ESP32-S3
- TRIG HC-SR04 → GPIO5 ESP32-S3
- ECHO HC-SR04 → GPIO18 ESP32-S3 (melalui pembagi tegangan)

> ⚠️ **Catatan Penting:**  
> HC-SR04 bekerja dengan tegangan 5V, tapi pin ECHO mengeluarkan sinyal 5V yang dapat merusak pin GPIO ESP32-S3 yang bekerja pada 3.3V. Gunakan pembagi tegangan (resistor 1K dan 2K) untuk menurunkan tegangan dari 5V ke 3.3V.

**Contoh Kode HC-SR04:**

```cpp
/*
 * ESP32-S3 dengan Sensor Ultrasonik HC-SR04
 * --------------------------------------
 * Program ini mengukur jarak menggunakan sensor ultrasonik HC-SR04
 * dan menampilkan hasilnya dalam centimeter dan inch.
 */

// Definisikan pin
#define TRIG_PIN 5
#define ECHO_PIN 18

// Konstanta untuk konversi
#define SOUND_SPEED 0.034  // Kecepatan suara dalam cm/mikrodetik
#define CM_TO_INCH 0.393701  // Faktor konversi cm ke inch

// Variabel untuk filter
#define READINGS_COUNT 5
float distanceReadings[READINGS_COUNT];
int readIndex = 0;
float totalDistance = 0;
float averageDistance = 0;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 HC-SR04 Ultrasonic Sensor Test");
  
  // Konfigurasi pin
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  
  // Inisialisasi array pembacaan dengan 0
  for (int i = 0; i < READINGS_COUNT; i++) {
    distanceReadings[i] = 0;
  }
}

float measureDistance() {
  // Reset pin TRIG
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  
  // Set pin TRIG HIGH selama 10 mikrodetik
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  // Baca pin ECHO, dan konversi waktu perjalanan ke jarak
  long duration = pulseIn(ECHO_PIN, HIGH);
  
  // Hitung jarak
  float distance = duration * SOUND_SPEED / 2;
  
  return distance;
}

void loop() {
  // Ukur jarak
  float currentDistance = measureDistance();
  
  // Validasi pengukuran (cek nilai di luar rentang realistis)
  if (currentDistance <= 0 || currentDistance > 400) {
    Serial.println("Pengukuran di luar rentang atau error");
    delay(100);
    return;
  }
  
  // Update filter rata-rata bergerak
  totalDistance = totalDistance - distanceReadings[readIndex];
  distanceReadings[readIndex] = currentDistance;
  totalDistance = totalDistance + distanceReadings[readIndex];
  readIndex = (readIndex + 1) % READINGS_COUNT;
  averageDistance = totalDistance / READINGS_COUNT;
  
  // Konversi ke inch
  float distanceInch = averageDistance * CM_TO_INCH;
  
  // Tampilkan hasil
  Serial.print("Jarak: ");
  Serial.print(averageDistance, 1);
  Serial.print(" cm / ");
  Serial.print(distanceInch, 1);
  Serial.println(" inch");
  
  // Tambahkan interpretasi jarak
  if (averageDistance < 10) {
    Serial.println("Objek sangat dekat! Hati-hati!");
  } else if (averageDistance < 30) {
    Serial.println("Objek dekat");
  } else if (averageDistance < 100) {
    Serial.println("Objek dalam jangkauan sedang");
  } else {
    Serial.println("Objek jauh");
  }
  
  Serial.println();
  delay(500);  // Pengukuran setiap 0.5 detik
}
```

**Aplikasi HC-SR04:**
1. Deteksi jarak dan keberadaan objek
2. Sensor parkir kendaraan
3. Level air pada tangki
4. Deteksi kehadiran dan sistem keamanan
5. Robot navigasi dan penghindaran rintangan

### 2. PIR (Passive Infrared) Motion Sensor

Sensor PIR (Passive Infrared) mendeteksi gerakan dengan mendeteksi perubahan radiasi inframerah. Sensor ini ideal untuk mendeteksi pergerakan manusia dalam area tertentu.

<div align="center">
   <img src="https://github.com/user-attachments/assets/da03b288-73ec-4ce3-afb0-13e2c8230a1e" width="300">
  <p><em>Sensor gerak PIR</em></p>
</div>

**Karakteristik PIR:**
- Jangkauan deteksi tipikal: 3-7 meter
- Sudut deteksi: sekitar 110° x 70°
- Output digital: HIGH ketika gerakan terdeteksi
- Waktu delay dan sensitivitas dapat disesuaikan (pada beberapa model)
- Konsumsi daya rendah, cocok untuk aplikasi baterai

**Rangkaian dengan PIR:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/fd26720b-ab3a-4695-94fb-138a598bc4b3" width="300">
  <p><em>Rangkaian PIR dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Sensor PIR (HC-SR501 atau sejenisnya)
- LED untuk indikator (opsional)
- Resistor 220 ohm untuk LED (opsional)
- Kabel jumper

**Koneksi:**
- VCC PIR → 5V atau 3.3V ESP32-S3 (tergantung modul, kebanyakan bekerja dengan 3.3V)
- GND PIR → GND ESP32-S3
- OUT PIR → GPIO14 ESP32-S3
- (Opsional) LED Anoda → GPIO2 ESP32-S3 melalui resistor 220 ohm
- (Opsional) LED Katoda → GND ESP32-S3

**Contoh Kode PIR:**

```cpp
/*
 * ESP32-S3 dengan Sensor Gerak PIR
 * ------------------------------
 * Program ini mendeteksi gerakan menggunakan sensor PIR
 * dan memicu LED dan pesan serial saat gerakan terdeteksi.
 */

// Definisikan pin
#define PIR_PIN 14
#define LED_PIN 2

// Variabel untuk melacak status gerakan
bool motionDetected = false;
unsigned long detectionTime = 0;
unsigned long noMotionTime = 0;

// Konstanta untuk manajemen timing
#define MOTION_TIMEOUT 30000  // 30 detik timeout setelah tidak ada gerakan
#define DEBOUNCE_TIME 1000    // Waktu debounce 1 detik

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 PIR Motion Sensor Test");
  
  // Konfigurasi pin
  pinMode(PIR_PIN, INPUT);
  pinMode(LED_PIN, OUTPUT);
  
  // Inisialisasi LED ke OFF
  digitalWrite(LED_PIN, LOW);
  
  Serial.println("Menunggu sensor PIR menstabilkan...");
  delay(3000);  // Berikan waktu untuk sensor menstabilkan
  Serial.println("Sensor PIR siap!");
}

void loop() {
  // Baca status sensor PIR
  int pirState = digitalRead(PIR_PIN);
  unsigned long currentTime = millis();
  
  // Jika gerakan terdeteksi
  if (pirState == HIGH) {
    // Jika ini adalah deteksi baru atau setelah debounce
    if (!motionDetected || (currentTime - detectionTime > DEBOUNCE_TIME)) {
      motionDetected = true;
      detectionTime = currentTime;
      Serial.println("Gerakan terdeteksi!");
      
      // Nyalakan LED
      digitalWrite(LED_PIN, HIGH);
    }
    // Reset timer "tidak ada gerakan"
    noMotionTime = currentTime;
  } 
  else {
    // Jika tidak ada gerakan terdeteksi untuk periode waktu tertentu
    if (motionDetected && (currentTime - noMotionTime > MOTION_TIMEOUT)) {
      motionDetected = false;
      Serial.println("Tidak ada gerakan selama " + String(MOTION_TIMEOUT/1000) + " detik.");
      
      // Matikan LED
      digitalWrite(LED_PIN, LOW);
    }
  }
  
  delay(100);  // Sedikit delay untuk stabilitas
}
```

> 💡 **Catatan tentang Penyesuaian Sensor PIR:**  
> Banyak modul PIR memiliki dua potensiometer untuk menyesuaikan:
> 1. **Sensitivitas** - Mengontrol seberapa sensitif sensor terhadap gerakan
> 2. **Delay time** - Berapa lama output tetap HIGH setelah gerakan terdeteksi

**Aplikasi PIR:**
1. Lampu otomatis yang menyala saat ada gerakan
2. Sistem keamanan dan deteksi intrusi
3. Pemantauan aktivitas dalam ruangan
4. Otomasi rumah berbasis kehadiran
5. Penghematan energi (mematikan perangkat saat tidak ada aktivitas)

### 3. MPU6050 (Akselerometer dan Giroskop 6-Axis)

MPU6050 adalah sensor gerakan 6-axis populer yang menggabungkan akselerometer 3-axis dan giroskop 3-axis dalam satu chip. Sensor ini ideal untuk mendeteksi orientasi, percepatan, dan gerakan rotasi.

<div align="center">
   <img src="https://github.com/user-attachments/assets/9962b7b7-baef-42c9-99af-6dfd36e8e76c" width="300">
  <p><em>Sensor MPU6050</em></p>
</div>

**Fitur MPU6050:**
- Akselerometer 3-axis dengan rentang skala ±2g, ±4g, ±8g, dan ±16g
- Giroskop 3-axis dengan rentang skala ±250, ±500, ±1000, dan ±2000°/s
- Sensor suhu terintegrasi
- Digital Motion Processor™ (DMP) untuk pemrosesan data gerakan kompleks
- Antarmuka I2C (alamat 0x68 atau 0x69)
- Tegangan operasi: 3-5V (biasanya 3.3V)

**Rangkaian dengan MPU6050:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/44a1b1ee-a96a-4c6c-8fee-84776aaede20" width="300">
  <p><em>Rangkaian MPU6050 dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Modul sensor MPU6050
- Kabel jumper

**Koneksi:**
- VCC MPU6050 → 3.3V ESP32-S3
- GND MPU6050 → GND ESP32-S3
- SCL MPU6050 → GPIO22 ESP32-S3 (default I2C SCL)
- SDA MPU6050 → GPIO21 ESP32-S3 (default I2C SDA)
- INT MPU6050 → tidak terhubung (opsional, dapat dihubungkan ke pin GPIO untuk interrupt)
- AD0 MPU6050 → GND ESP32-S3 (untuk alamat I2C 0x68) atau 3.3V (untuk alamat 0x69)

**Contoh Kode MPU6050:**

```cpp
/*
 * ESP32-S3 dengan Sensor MPU6050
 * ----------------------------
 * Program ini membaca data akselerometer dan giroskop dari MPU6050
 * dan menampilkan hasilnya melalui Serial Monitor.
 */

#include <Wire.h>
#include <Adafruit_MPU6050.h>
#include <Adafruit_Sensor.h>

// Inisialisasi objek MPU6050
Adafruit_MPU6050 mpu;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  while (!Serial) delay(10);  // Akan menunggu hingga serial siap
  
  Serial.println("ESP32-S3 MPU6050 Test");
  
  // Inisialisasi I2C
  Wire.begin();
  
  // Inisialisasi MPU6050
  if (!mpu.begin()) {
    Serial.println("Gagal menemukan chip MPU6050");
    while (1) {
      delay(10);
    }
  }
  
  Serial.println("MPU6050 ditemukan!");
  
  // Konfigurasi sensor
  mpu.setAccelerometerRange(MPU6050_RANGE_8_G);
  mpu.setGyroRange(MPU6050_RANGE_500_DEG);
  mpu.setFilterBandwidth(MPU6050_BAND_21_HZ);
  
  // Tampilkan konfigurasi
  Serial.print("Rentang Akselerometer: ±");
  Serial.print(mpu.getAccelerometerRange() * 2);
  Serial.println("G");
  
  Serial.print("Rentang Giroskop: ±");
  Serial.print(mpu.getGyroRange() * 250);
  Serial.println(" °/s");
  
  Serial.print("Filter Bandwidth: ");
  Serial.print(mpu.getFilterBandwidth());
  Serial.println(" Hz");
}

void loop() {
  // Baca sensor
  sensors_event_t a, g, temp;
  mpu.getEvent(&a, &g, &temp);
  
  // Tampilkan data akselerometer
  Serial.println("---------------------");
  Serial.print("Akselerometer - X: ");
  Serial.print(a.acceleration.x);
  Serial.print(" m/s², Y: ");
  Serial.print(a.acceleration.y);
  Serial.print(" m/s², Z: ");
  Serial.print(a.acceleration.z);
  Serial.println(" m/s²");
  
  // Tampilkan data giroskop
  Serial.print("Giroskop - X: ");
  Serial.print(g.gyro.x);
  Serial.print(" rad/s, Y: ");
  Serial.print(g.gyro.y);
  Serial.print(" rad/s, Z: ");
  Serial.print(g.gyro.z);
  Serial.println(" rad/s");
  
  // Tampilkan data suhu
  Serial.print("Suhu: ");
  Serial.print(temp.temperature);
  Serial.println(" °C");
  
  // Deteksi gerakan sederhana
  float totalAccel = sqrt(a.acceleration.x * a.acceleration.x + 
                          a.acceleration.y * a.acceleration.y + 
                          a.acceleration.z * a.acceleration.z);
                          
  Serial.print("Total Akselerasi: ");
  Serial.print(totalAccel);
  Serial.println(" m/s²");
  
  if (totalAccel > 12) {  // ~1.2g (termasuk gravitasi ~9.8 m/s²)
    Serial.println("⚠️ GERAKAN SIGNIFIKAN TERDETEKSI!");
  } else if (totalAccel < 8) {  // Kurang dari ~0.8g
    Serial.println("⚠️ KEMUNGKINAN JATUH ATAU JATUH BEBAS!");
  }
  
  // Deteksi orientasi sederhana
  Serial.print("Posisi: ");
  if (a.acceleration.z > 7) {
    Serial.println("Datar menghadap ke atas");
  } else if (a.acceleration.z < -7) {
    Serial.println("Datar menghadap ke bawah");
  } else if (a.acceleration.x > 7) {
    Serial.println("Vertikal");
  } else if (a.acceleration.x < -7) {
    Serial.println("Terbalik");
  } else if (a.acceleration.y > 7) {
    Serial.println("Miring ke kanan");
  } else if (a.acceleration.y < -7) {
    Serial.println("Miring ke kiri");
  } else {
    Serial.println("Orientasi kompleks");
  }
  
  Serial.println();
  delay(500);
}
```

**Aplikasi MPU6050:**
1. Deteksi orientasi perangkat
2. Pelacakan gerakan dan aktivitas
3. Deteksi jatuh untuk aplikasi kesehatan
4. Kontrol gestur untuk perangkat
5. Stabilisasi kamera atau drone
6. Pengembangan wearable untuk pemantauan olahraga

### 4. ESP32-S3 Touch Sensor (Kapasitif)

ESP32-S3 memiliki sensor sentuh kapasitif terintegrasi yang memungkinkan Anda membuat antarmuka sentuh tanpa komponen tambahan. Sensor sentuh memungkinkan deteksi sentuhan manusia pada permukaan konduktif yang terhubung ke pin GPIO tertentu.

**Fitur Sensor Sentuh ESP32-S3:**
- Mendukung hingga 14 saluran sentuh (Touch0 - Touch13)
- Sensitivitas dapat diatur
- Mendukung mode sleep untuk aplikasi hemat energi
- Dapat digunakan untuk wake-up dari deep sleep
- Fungsionalitas filter noise terintegrasi

**Rangkaian untuk Sensor Sentuh:**

<div align="center">
   <img src="https://github.com/user-attachments/assets/3d67729a-ddf4-4d25-8e5c-111e7877dd32" width="300">
  <p><em>Rangkaian Touch Sensor dengan ESP32-S3</em></p>
</div>

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Konduktor (bisa berupa papan PCB, kertas aluminium, atau bahan konduktif lainnya)
- Resistor 1M ohm (opsional, untuk meningkatkan stabilitas)
- LED untuk indikator (opsional)
- Resistor 220 ohm untuk LED (opsional)
- Kabel jumper

**Koneksi:**
- Konduktor → GPIO4 ESP32-S3 (merupakan channel touch0)
- (Opsional) Resistor 1M ohm antara pin sentuh dan ground
- (Opsional) LED Anoda → GPIO2 ESP32-S3 melalui resistor 220 ohm
- (Opsional) LED Katoda → GND ESP32-S3

**Pemetaan Pin Sentuh ESP32-S3:**

| GPIO | Channel Sentuh |
|------|---------------|
| GPIO1 | TOUCH1 |
| GPIO2 | TOUCH2 |
| GPIO3 | TOUCH3 |
| GPIO4 | TOUCH4 |
| GPIO5 | TOUCH5 |
| GPIO6 | TOUCH6 |
| GPIO7 | TOUCH7 |
| GPIO8 | TOUCH8 |
| GPIO9 | TOUCH9 |
| GPIO10 | TOUCH10 |
| GPIO11 | TOUCH11 |
| GPIO12 | TOUCH12 |
| GPIO13 | TOUCH13 |
| GPIO14 | TOUCH14 |

**Contoh Kode Sensor Sentuh:**

```cpp
/*
 * ESP32-S3 Touch Sensor
 * ------------------
 * Program ini menunjukkan cara menggunakan sensor sentuh kapasitif 
 * terintegrasi ESP32-S3 dan mengontrol LED berdasarkan sentuhan.
 */

// Definisikan pin
#define TOUCH_PIN 4    // GPIO4 (TOUCH4)
#define LED_PIN 2      // Pin untuk LED indikator

// Array untuk menyimpan pembacaan sentuh sebelumnya untuk kalibrasi
#define SAMPLES 10
uint16_t touchReadings[SAMPLES];
uint16_t readingIndex = 0;
uint16_t touchThreshold = 0;
bool isCalibrated = false;

// Variabel untuk status sentuh
bool isTouched = false;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("ESP32-S3 Touch Sensor Test");
  
  // Konfigurasi LED pin
  pinMode(LED_PIN, OUTPUT);
  
  Serial.println("Memulai kalibrasi sensor sentuh...");
  Serial.println("Pastikan tidak ada yang menyentuh sensor saat kalibrasi");
  delay(2000);  // Berikan waktu sebelum kalibrasi dimulai
  
  // Kalibrasi sensor sentuh
  calibrateTouch();
}

void calibrateTouch() {
  // Ambil beberapa pembacaan untuk kalibrasi
  uint32_t total = 0;
  
  for (int i = 0; i < SAMPLES; i++) {
    touchReadings[i] = touchRead(TOUCH_PIN);
    total += touchReadings[i];
    delay(100);
  }
  
  // Hitung rata-rata dan tetapkan threshold
  uint16_t average = total / SAMPLES;
  touchThreshold = average * 0.7;  // 70% dari nilai rata-rata
  
  Serial.print("Nilai sentuh rata-rata (tidak disentuh): ");
  Serial.println(average);
  Serial.print("Threshold sentuh: ");
  Serial.println(touchThreshold);
  
  isCalibrated = true;
  Serial.println("Kalibrasi selesai! Sensor siap digunakan.");
}

void loop() {
  // Baca nilai sensor sentuh
  uint16_t touchValue = touchRead(TOUCH_PIN);
  
  // Tampilkan nilai sentuh
  Serial.print("Nilai sensor sentuh: ");
  Serial.print(touchValue);
  Serial.print(" | Threshold: ");
  Serial.println(touchThreshold);
  
  // Deteksi sentuhan jika nilai di bawah threshold
  // Nilai lebih rendah = sentuhan terdeteksi
  if (touchValue < touchThreshold && !isTouched) {
    // Sentuhan baru terdeteksi
    isTouched = true;
    digitalWrite(LED_PIN, HIGH);  // Nyalakan LED
    Serial.println("👆 Sentuhan terdeteksi!");
  } 
  else if (touchValue >= touchThreshold && isTouched) {
    // Sentuhan selesai
    isTouched = false;
    digitalWrite(LED_PIN, LOW);   // Matikan LED
    Serial.println("👋 Sentuhan dilepas");
  }
  
  delay(100);  // Sedikit delay untuk stabilitas
}
```

**Aplikasi Sensor Sentuh ESP32-S3:**
1. Tombol sentuh tanpa bagian mekanis
2. Slider sentuh untuk kontrol volume atau kecerahan
3. Keypad untuk input password atau kontrol
4. Interface sentuh tahan air
5. Wake-up perangkat dari mode sleep dengan sentuhan

---

## 🔄 Pertemuan 4: Integrasi Multi-Sensor

Integrasi beberapa sensor adalah kunci untuk membuat aplikasi IoT yang lebih kompleks dan fungsional. Pada bagian ini, kita akan melihat bagaimana menggabungkan beberapa sensor yang telah kita pelajari untuk membuat sistem pemantauan lingkungan yang komprehensif.

### Stasiun Pemantauan Lingkungan dan Keamanan

Proyek ini menggabungkan sensor lingkungan (BME680, BH1750) dan sensor gerak (PIR, HC-SR04) untuk:
1. Memantau kondisi lingkungan (suhu, kelembaban, tekanan, kualitas udara, cahaya)
2. Mendeteksi keberadaan dan gerakan dalam ruangan
3. Menyimpan data dalam log dan menampilkannya melalui Serial Monitor

**Komponen yang dibutuhkan:**
- ESP32-S3 Development Board
- Sensor BME680
- Sensor BH1750
- Sensor PIR
- Sensor HC-SR04
- LED untuk indikator (3 warna berbeda)
- Resistor 220 ohm untuk LED
- Buzzer (opsional)
- Kabel jumper

**Koneksi:**
- BME680 dan BH1750 menggunakan bus I2C yang sama (GPIO21/GPIO22)
- PIR → GPIO14
- HC-SR04: TRIG → GPIO5, ECHO → GPIO18 (melalui pembagi tegangan)
- LED Merah → GPIO25
- LED Kuning → GPIO26
- LED Hijau → GPIO27
- Buzzer → GPIO12 (opsional)

**Contoh Kode Integrasi:**

```cpp
/*
 * ESP32-S3 Integrated Environmental and Security Monitoring Station
 * ----------------------------------------------------------------
 * Program ini mengintegrasikan beberapa sensor untuk pemantauan lingkungan dan keamanan:
 * - BME680: Suhu, kelembaban, tekanan, kualitas udara
 * - BH1750: Intensitas cahaya
 * - PIR: Deteksi gerakan
 * - HC-SR04: Pengukuran jarak
 */

#include <Wire.h>
#include <Adafruit_Sensor.h>
#include <Adafruit_BME680.h>
#include <BH1750.h>

// Definisikan pin
#define PIR_PIN 14
#define TRIG_PIN 5
#define ECHO_PIN 18
#define LED_RED 25
#define LED_YELLOW 26
#define LED_GREEN 27
#define BUZZER_PIN 12

// Konstanta
#define SOUND_SPEED 0.034
#define SEALEVELPRESSURE_HPA (1013.25)
#define DISTANCE_THRESHOLD 50  // cm

// Inisialisasi objek sensor
Adafruit_BME680 bme;
BH1750 lightMeter;

// Status keamanan
bool motionDetected = false;
bool proximityAlert = false;
bool environmentalAlert = false;

void setup() {
  // Inisialisasi komunikasi serial
  Serial.begin(115200);
  Serial.println("\nESP32-S3 Multi-Sensor Integration");
  
  // Inisialisasi I2C
  Wire.begin();
  
  // Inisialisasi pin
  pinMode(PIR_PIN, INPUT);
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  pinMode(LED_RED, OUTPUT);
  pinMode(LED_YELLOW, OUTPUT);
  pinMode(LED_GREEN, OUTPUT);
  pinMode(BUZZER_PIN, OUTPUT);
  
  // Matikan semua LED dan buzzer
  digitalWrite(LED_RED, LOW);
  digitalWrite(LED_YELLOW, LOW);
  digitalWrite(LED_GREEN, LOW);
  digitalWrite(BUZZER_PIN, LOW);
  
  // Inisialisasi BME680
  Serial.print("Initializing BME680... ");
  if (!bme.begin(0x76)) {
    if (!bme.begin(0x77)) {
      Serial.println("Failed! Check wiring.");
    } else {
      Serial.println("Success at address 0x77!");
    }
  } else {
    Serial.println("Success at address 0x76!");
  }
  
  if (bme.begin()) {
    // Set up oversampling and filter initialization
    bme.setTemperatureOversampling(BME680_OS_8X);
    bme.setHumidityOversampling(BME680_OS_2X);
    bme.setPressureOversampling(BME680_OS_4X);
    bme.setIIRFilterSize(BME680_FILTER_SIZE_3);
    bme.setGasHeater(320, 150); // 320°C for 150 ms
  }
  
  // Inisialisasi BH1750
  Serial.print("Initializing BH1750... ");
  if (lightMeter.begin()) {
    Serial.println("Success!");
  } else {
    Serial.println("Failed! Check wiring.");
  }
  
  Serial.println("System ready!");
  
  // Nyalakan LED hijau untuk menunjukkan sistem siap
  digitalWrite(LED_GREEN, HIGH);
  delay(1000);
  digitalWrite(LED_GREEN, LOW);
}

void loop() {
  // Timestamp untuk log
  unsigned long currentMillis = millis();
  
  // ===== Pemantauan Lingkungan =====
  
  // -- BME680 --
  Serial.println("\n===== ENVIRONMENTAL MONITORING =====");
  
  if (bme.begin()) {
    if (bme.performReading()) {
      float temperature = bme.temperature;
      float humidity = bme.humidity;
      float pressure = bme.pressure / 100.0;
      float gas = bme.gas_resistance / 1000.0;
      
      Serial.println("----- BME680 Readings -----");
      Serial.print("Temperature: "); Serial.print(temperature, 1); Serial.println(" °C");
      Serial.print("Humidity: "); Serial.print(humidity, 1); Serial.println(" %");
      Serial.print("Pressure: "); Serial.print(pressure, 1); Serial.println(" hPa");
      Serial.print("Gas: "); Serial.print(gas, 1); Serial.println(" kOhms");
      
      // Analisis kualitas udara
      String airQuality;
      if (gas > 50) airQuality = "Excellent";
      else if (gas > 20) airQuality = "Good";
      else if (gas > 10) airQuality = "Fair";
      else if (gas > 5) airQuality = "Poor";
      else airQuality = "Very Poor";
      
      Serial.print("Air Quality: "); Serial.println(airQuality);
      
      // Set alert jika kondisi ekstrem
      environmentalAlert = (temperature > 35.0 || temperature < 5.0 || 
                           humidity > 80.0 || 
                           gas < 5.0);
    } else {
      Serial.println("Failed to read BME680");
    }
  }
  
  // -- BH1750 --
  if (lightMeter.begin()) {
    float lux = lightMeter.readLightLevel();
    
    Serial.println("----- BH1750 Readings -----");
    Serial.print("Light: "); Serial.print(lux); Serial.println(" lx");
    
    // Analisis kondisi cahaya
    String lightCondition;
    if (lux < 10) lightCondition = "Very Dark";
    else if (lux < 50) lightCondition = "Dark";
    else if (lux < 200) lightCondition = "Dim";
    else if (lux < 500) lightCondition = "Normal Indoor";
    else if (lux < 1000) lightCondition = "Bright";
    else lightCondition = "Very Bright";
    
    Serial.print("Light Condition: "); Serial.println(lightCondition);
  }
  
  // ===== SECURITY MONITORING =====
  Serial.println("\n===== SECURITY MONITORING =====");
  
  // -- PIR Motion Sensor --
  int pirState = digitalRead(PIR_PIN);
  if (pirState == HIGH) {
    if (!motionDetected) {
      Serial.println("Motion detected!");
      motionDetected = true;
    }
  } else {
    if (motionDetected) {
      Serial.println("Motion stopped");
      motionDetected = false;
    }
  }
  
  // -- HC-SR04 Ultrasonic Sensor --
  // Reset TRIG pin
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  
  // Set TRIG pin HIGH for 10 microseconds
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  // Read ECHO pin
  long duration = pulseIn(ECHO_PIN, HIGH);
  
  // Calculate distance
  float distance = duration * SOUND_SPEED / 2;
  
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // Check if object is too close
  proximityAlert = (distance < DISTANCE_THRESHOLD && distance > 0);
  if (proximityAlert) {
    Serial.println("⚠️ Proximity alert: Object too close!");
  }
  
  // ===== ALERT HANDLING =====
  // Update LED indicators
  digitalWrite(LED_RED, (proximityAlert || environmentalAlert));
  digitalWrite(LED_YELLOW, motionDetected);
  digitalWrite(LED_GREEN, !(proximityAlert || environmentalAlert || motionDetected));
  
  // Sound buzzer if there's an alert
  digitalWrite(BUZZER_PIN, (proximityAlert || environmentalAlert));
  
  // Print overall status
  Serial.println("\n===== SYSTEM STATUS =====");
  Serial.print("Environment: "); 
  Serial.println(environmentalAlert ? "⚠️ ALERT" : "✓ Normal");
  
  Serial.print("Security: "); 
  Serial.println((motionDetected || proximityAlert) ? "⚠️ ALERT" : "✓ Normal");
  
  delay(2000);  // Update every 2 seconds
}
```

---

## 📝 Tugas dan Latihan

### Latihan 1: Sensor BME680
Buatlah sistem pemantauan kualitas udara dengan BME680 yang dapat menampilkan status kualitas udara (Sangat Baik, Baik, Sedang, Buruk, Sangat Buruk) berdasarkan pembacaan gas VOC, dan menyalakan LED yang berbeda untuk setiap status.

### Latihan 2: Cahaya dan Lingkungan
Integrasikan BH1750 dan LDR Digital untuk membuktikan akurasi pembacaan sensor BH1750. Buatlah laporan perbandingan keduanya dengan pengukuran dalam berbagai kondisi cahaya.

### Latihan 3: Deteksi Gerakan dan Jarak
Buatlah sistem alarm sederhana yang menggunakan PIR dan HC-SR04. Alarm akan berbunyi ketika:
- Gerakan terdeteksi oleh PIR DAN
- Objek berada pada jarak kurang dari 50cm (terdeteksi oleh HC-SR04)

### Latihan 4: Pendeteksi Gerakan dengan MPU6050
Gunakan MPU6050 untuk mendeteksi gerakan spesifik (misalnya "ketukan", "penggoyangan", atau "rotasi"). Sistem akan menyalakan LED yang berbeda untuk setiap jenis gerakan yang terdeteksi.

### Proyek Mini: Stasiun Pemantauan Multifungsi
Integrasi minimal 3 sensor (pilih dari BME680, BH1750, PIR, HC-SR04, MPU6050, Touch Sensor) untuk membuat stasiun pemantauan multifungsi. Sistem harus menampilkan semua data sensor dan membuat keputusan atau tindakan berdasarkan kombinasi pembacaan dari berbagai sensor.

---

## 📚 Referensi dan Bacaan Tambahan

### Buku Referensi
1. Schwartz, M. (2023). *Internet of Things with ESP32: Building Connected Devices with ESP32 Microcontrollers*. Apress. Bab 4-6.
2. Blum, J. (2022). *Exploring Arduino: Tools and Techniques for Engineering Wizardry* (2nd Edition). Wiley. Bab 8-10.
3. Kurniawan, A. (2023). *Internet of Things Projects with ESP32: Building Web Servers, MQTT, and Sensors*. Apress.

### Datasheet dan Dokumentasi
1. Bosch. (2022). *BME680 Datasheet*. [https://www.bosch-sensortec.com/products/environmental-sensors/gas-sensors/bme680/](https://www.bosch-sensortec.com/products/environmental-sensors/gas-sensors/bme680/)
2. ROHM Semiconductor. (2021). *BH1750FVI Datasheet*. [https://www.rohm.com/products/sensors-mems/ambient-light-sensor-ics/bh1750fvi-product](https://www.rohm.com/products/sensors-mems/ambient-light-sensor-ics/bh1750fvi-product)
3. InvenSense. (2020). *MPU-6050 Datasheet*. [https://invensense.tdk.com/products/motion-tracking/6-axis/mpu-6050/](https://invensense.tdk.com/products/motion-tracking/6-axis/mpu-6050/)
4. Espressif Systems. (2024). *ESP32-S3 Technical Reference Manual*. [https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf)

### Tutorial dan Materi Online
1. ElectroDragon. (2023). "BME680 Air Quality Monitoring with ESP32". [https://www.electrodragon.com/w/BME680](https://www.electrodragon.com/w/BME680)
2. Random Nerd Tutorials. (2023). "ESP32 with HC-SR04 Ultrasonic Sensor". [https://randomnerdtutorials.com/esp32-hc-sr04-ultrasonic-arduino/](https://randomnerdtutorials.com/esp32-hc-sr04-ultrasonic-arduino/)
3. Last Minute Engineers. (2022). "How PIR Motion Sensors Work". [https://lastminuteengineers.com/pir-sensor-arduino-tutorial/](https://lastminuteengineers.com/pir-sensor-arduino-tutorial/)
4. DFRobot. (2023). "MPU6050 Detailed Tutorial". [https://wiki.dfrobot.com/How_to_Use_a_Three-Axis_Accelerometer_for_Tilt_Sensing](https://wiki.dfrobot.com/How_to_Use_a_Three-Axis_Accelerometer_for_Tilt_Sensing)
5. Adafruit Learning System. (2022). "BH1750 Light Sensor Guide". [https://learn.adafruit.com/adafruit-bh1750-ambient-light-sensor](https://learn.adafruit.com/adafruit-bh1750-ambient-light-sensor)

### Library Documentation
1. Adafruit BME680 Library: [https://github.com/adafruit/Adafruit_BME680](https://github.com/adafruit/Adafruit_BME680)
2. BH1750 Library: [https://github.com/claws/BH1750](https://github.com/claws/BH1750)
3. Adafruit MPU6050 Library: [https://github.com/adafruit/Adafruit_MPU6050](https://github.com/adafruit/Adafruit_MPU6050)
4. NewPing Library (for HC-SR04): [https://github.com/microflo/NewPing](https://github.com/microflo/NewPing)

---

<div align="center">
  
  **[⬅️ Kembali ke Modul 1](../module-1/README.md) | [Lanjut ke Modul 3 ➡️](../module-3/README.md)**
  
  *Developed with ❤️ for IoT Education*
  
</div>

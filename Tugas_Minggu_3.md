# KONSEP JARINGAN
## Latihan Konsep Dasar Jaringan Komputer
**DOSEN:** 
Dr. Ferry Astika Saputra, ST, M.Sc. (197708232001121002)

**PENYUSUN:**
Zakil Muannisi (3125600032)

**PROGRAM STUDI SARJANA TERAPAN TEKNIK INFORMATIKA**

**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**

---


## Level A — Ingatan dan Pemahaman

### 1. Fungsi Utama Physical Layer dan Hubungannya dengan Data Link Layer
* Fungsi Utama: Mentransmisikan bit data mentah (0 dan 1) melalui media komunikasi fisik dalam bentuk sinyal listrik, gelombang radio, atau pulsa cahaya.
* Hubungan dengan Data Link Layer: Data Link mengubah bingkai data (frame) menjadi urutan bit, lalu menyerahkannya ke Physical Layer untuk diubah menjadi sinyal fisik. Penerima melakukan proses sebaliknya.

### 2. Perbedaan Data/Sinyal Analog dan Digital
* Data Analog: Informasi berkesinambungan yang nilainya bervariasi secara kontinu (contoh: gelombang suara).
* Data Digital: Informasi diskrit yang bernilai terputus-putus (contoh: angka 0 dan 1 pada file teks).
* Sinyal Analog: Gelombang elektromagnetik kontinu dengan variasi amplitudo dan frekuensi tanpa batas rentang nilai.
* Sinyal Digital: Pulsa tegangan atau cahaya diskrit yang mewakili dua tingkat kondisi (High/Low).

### 3. Amplitudo, Frekuensi, Fase, dan Panjang Gelombang
* Amplitudo: Tinggi atau besarnya nilai puncak suatu gelombang.
* Frekuensi: Jumlah siklus gelombang penuh yang terjadi dalam satu detik (Hertz).
* Fase: Kedudukan atau sudut acuan gelombang pada titik waktu tertentu relatif terhadap awal siklus.
* Panjang Gelombang: Jarak fisik yang ditempuh oleh satu siklus gelombang penuh.

### 4. Bandwidth dalam Hertz vs Laju Bit
* Bandwidth (Hz): Rentang frekuensi yang dapat dilalui oleh media transmisi fisik (frekuensi tertinggi dikurangi frekuensi terendah).
* Laju Bit (bps): Jumlah bit data asli yang dapat dikirimkan melalui jalur komunikasi dalam waktu satu detik.

### 5. Bit Rate vs Symbol Rate
* Bit Rate: Jumlah bit (0/1) yang ditransmisikan per detik (bps).
* Symbol Rate (Baud Rate): Jumlah perubahan sinyal fisik atau simbol yang dikirim per detik. Satu simbol dapat membawa lebih dari satu bit.

### 6. Redaman, Distorsi, Noise, dan Interferensi
* Redaman (Attenuation): Pelemahan kekuatan sinyal saat merambat makin jauh melalui media fisik.
* Distorsi: Perubahan bentuk gelombang sinyal akibat perbedaan respon frekuensi media transmisi.
* Noise: Sinyal acak yang tidak diinginkan dari luar yang bercampur dengan sinyal asli (contoh: thermal noise).
* Interferensi: Gangguan sinyal yang disebabkan oleh pemancar radio atau perangkat elektronik lain pada frekuensi setara.

### 7. Perbedaan dB dan dBm
* dB (Decibel): Satuan nisbi (relatif) yang digunakan untuk mengukur rasio penguatan atau pelemahan daya antara dua titik.
* dBm: Satuan mutlak (absolut) pengukuran daya dengan nilai acuan 1 milliwatt (0 dBm = 1 mW).

### 8. Mengapa Manchester Disebut Self-Clocking?
Karena pengodean Manchester selalu memiliki transisi sinyal di tengah-tengah setiap periode bit (transisi tinggi-ke-rendah atau rendah-ke-tinggi). Transisi wajib ini berfungsi sebagai sinyal detak (clock) agar penerima tetap tersinkronisasi tanpa memerlukan jalur clock terpisah.

### 9. Perbedaan Block Coding, Scrambling, dan Enkripsi
* Block Coding: Menambahkan bit redundant pada urutan data untuk membantu deteksi dan koreksi kesalahan (contoh: 4B/5B).
* Scrambling: Mengacak urutan bit sinyal untuk mencegah munculnya deretan angka 0 atau 1 yang terlalu panjang agar sinkronisasi clock terjaga.
* Enkripsi: Mengubah isi payload data menggunakan kunci rahasia agar tidak dapat dibaca oleh pihak yang tidak sah.

### 10. Karakteristik Utama Media Kabel & Serat Optik
* Twisted Pair: Kabel tembaga terpilin, murah, jarak terbatas 100 meter, rentan terhadap gangguan elektromagnetik.
* Coaxial: Kabel tembaga berpelindung ganda, memiliki shielding lebih baik dibanding twisted pair, digunakan untuk TV kabel dan jaringan lama.
* Multimode Fiber (MMF): Serat optik dengan inti besar, menggunakan LED/VCSEL, jarak pendek (<2 km), biaya transceiver relatif murah.
* Single-mode Fiber (SMF): Serat optik dengan inti sangat kecil, menggunakan transmiter laser, jarak sangat jauh (>10 km) tanpa degradasi dispersion tinggi.

---

## Level B — Penerapan dan Analisis

### 1. Perhitungan Sinyal 8 Keadaan
* Jumlah bit per simbol: log2(8) = 3 bit per simbol.
* Bit rate pada 10 Mbaud: 10 MBaud x 3 bit = 30 Mbps.

### 2. Batas Nyquist Kanal Ideal
* Rumus Nyquist: Bit Rate = 2 x Bandwidth x log2(L)
* Perhitungan: 2 x 5.000 Hz x log2(8) = 10.000 x 3 = 30.000 bps (30 kbps).

### 3. Konversi SNR dan Kapasitas Shannon
* Konversi SNR 30 dB ke linear: 10^(30/10) = 1.000.
* Kapasitas Shannon: C = Bandwidth x log2(1 + SNR_linear)
* Perhitungan: 3.000 x log2(1 + 1.000) ≈ 3.000 x 9.9657 = 29.897 bps (29,9 kbps).

### 4. Perubahan Daya dalam dB
* Rumus: 10 x log10(Daya_akhir / Daya_awal)
* Perhitungan: 10 x log10(25 / 100) = 10 x log10(0.25) = 10 x (-0.602) = -6,02 dB (terjadi penurunan daya sekitar 6 dB).

### 5. Hitung Daya Terima Link Radio
* Rumus: Pemancar + Gain Tx - Path/Cable Loss + Gain Rx
* Perhitungan: 20 dBm + 6 dBi - 92 dB + 3 dBi = -63 dBm.

### 6. Hitung Daya Terima Link Optik
* Total Loss = Loss serat (3 dB) + 4 Konektor (4 x 0,5 dB = 2 dB) + 2 Splice (2 x 0,2 dB = 0,4 dB) = 5,4 dB.
* Daya Terima = Daya Kirim - Total Loss = -1 dBm - 5,4 dB = -6,4 dBm.

### 7. Mengapa 4096-QAM Tidak Selalu Digunakan?
Karena 4096-QAM memerlukan rasio Signal-to-Noise (SNR) yang sangat tinggi. Jika ada sedikit saja noise, interferensi, atau jarak yang agak jauh, penerima tidak bisa membedakan titik-titik konstelasi yang sangat rapat, sehingga sistem terpaksa turun (*fallback*) ke modulasi yang lebih rendah.

### 8. Memperlebar Kanal Wi-Fi vs Menambah Access Point (AP)
* Memperlebar Kanal (misal dari 20 MHz ke 80 MHz): Meningkatkan kecepatan maksimum per pengguna, namun menurunkan jumlah kanal yang bebas interferensi sehingga tidak cocok untuk area padat.
* Menambah Access Point (AP): Membagi jumlah pengguna ke dalam sel-sel kecil dengan kanal 20/40 MHz, mengurangi kontensi media, dan meningkatkan kapasitas total seluruh area.

### 9. Kasus Split Pair pada Kabel Ethernet
Split pair terjadi ketika urutan kabel salah dipasangkan tetapi jalur tembaganya tetap tersambung ujung-ke-ujung. Continuity tester biasa hanya mengecek kontinuitas arus searah (DC) sehingga dinyatakan lolos. Namun, karena pasangan pilinan tidak berada pada sinyal berpasangannya, timbul crosstalk induktif tinggi yang merusak sinyal AC data Ethernet.

### 10. Diagnosis Link Fiber Flapping
* Hipotesis: Ujung konektor optik kotor akibat debu saat pekerjaan patch panel, atau ada bending (tekukan) berlebih pada kabel patch cord.
* Urutan Pengujian Aman:
  1. Periksa visual dan jarak tekukan kabel serat optik.
  2. Gunakan Visual Fault Locator (VFL) lampu merah untuk cek kebocoran cahaya.
  3. Bersihkan permukaan konektor dengan alat pembersih serat optik khusus (fiber cleaner pen).
  4. Ukur redaman jalur menggunakan Optical Power Meter (OPM) atau OTDR.

---

## Level C — Evaluasi dan Sintesis

### 1. Evaluasi Media Pemhubung Gedung 600 Meter
* UTP: Tidak layak. Jarak maksimal UTP hanya 100 meter.
* Radio Point-to-Point: Biaya awal sedang dan fleksibel, tetapi performa rentan terpengaruh cuaca hujan lebat serta interferensi frekuensi.
* Single-mode Fiber (SMF): **Pilihan Terbaik.** Mampu melayani jarak jauh hingga puluhan kilometer, bandwidth sangat tinggi, tahan gangguan petir/listrik, dan lebih stabil untuk jangka panjang.

### 2. Rancangan Link Budget Konseptual
Komponen yang diteliti:
* Gain: Daya pancar pemancar (Tx Power), Gain antena pengirim (Tx Antenna Gain), Gain antena penerima (Rx Antenna Gain).
* Loss: Cable loss pengirim & penerima, Free Space Path Loss (FSPL), Obstacle/Fade Loss (pepohonan/bangunan), Rain Attenuation.
* Margin: Fade Margin (cadangan daya untuk mengantisipasi perubahan cuaca).

### 3. Hubungan Nyquist, Shannon, QAM, FEC, dan Adaptive Modulation
Nyquist menentukan batas kecepatan teoritis bebas noise berdasarkan jumlah tingkat sinyal, sementara Shannon menentukan batas absolut kapasitas informasi nyata akibat adanya noise (SNR). Untuk mendekati batas Shannon, digunakan teknik modulasi tingkat tinggi seperti QAM. Agar paket tidak rusak akibat noise, ditambahkan bit koreksi error (FEC). Jika kondisi lingkungan memburuk (SNR turun), sistem menggunakan Adaptive Modulation untuk menurunkan tingkat QAM secara otomatis agar koneksi tidak terputus.

### 4. Prosedur Survei dan Validasi Wi-Fi Ruang Kuliah (100 Mahasiswa)
1. Perancangan Kapasitas: Pasang minimal 2 Access Point (AP) untuk membagi beban 100 perangkat aktif.
2. Survei Lokasi (Passive Survey): Petakan kekuatan sinyal (minimal -65 dBm) dan pastikan tidak ada tumpang tindih kanal (co-channel interference).
3. Pengaturan Kanal: Gunakan lebar kanal 20 MHz pada frekuensi 5 GHz/6 GHz.
4. Validasi Beban (Active Stress Test): Hubungkan 100 perangkat simulasi untuk mengukur latensi, packet loss, dan throughput minimum per pengguna saat jam sibuk.

### 5. WDM vs Penambahan Kabel Serat Baru
* Capacity: WDM mampu meningkatkan kapasitas puluhan kali lipat pada satu helai serat; tarik kabel baru terbatas pada jumlah core kabel.
* Biaya: WDM memerlukan investasi awal perangkat optik (mux/demux) yang mahal tetapi hemat biaya penggalian; tarik serat baru sangat mahal pada biaya konstruksi/galian tanah.
* Failure Domain: Jika kabel fisik putus, WDM akan memutus seluruh gelombang cahaya sekaligus.
* Operasional: WDM lebih praktis dari sisi izin jalur karena menggunakan kabel fisik yang sudah ada.

### 6. Dampak Power over Ethernet (PoE) Berdaya Tinggi
* Cabling Bundle: Arus listrik menghasilkan panas di dalam ikatan kabel; membutuhkan kabel dengan standar gauge lebih tebal (misal Cat6A) agar tidak terkelupas/terbakar.
* Switch & UPS: Membutuhkan Power Supply Unit (PSU) switch berkapasitas besar (PoE budget tinggi) serta kapasitas cadangan baterai UPS yang lebih besar.
* Keselamatan: Risiko loncatan bunga api listrik saat mencabut kabel RJ45 dalam kondisi aktif (*sparking*).

### 7. Rancangan Redundansi Backbone Terpisah Total
1. Jalur Fisik: Dua jalur kabel optik ditanam melalui dua rute jalan yang berbeda (*diverse routing*).
2. Risiko Penggalian: Satu jalur ditarik lewat udara/tiang, satu jalur lain ditanam di bawah tanah pada kedalaman standar dengan pipa pelindung.
3. Catu Daya: Perangkat jaringan terhubung ke dua sumber listrik terpisah (PLN + Generator) menggunakan dua PSU terpisah (Redundant Power Supply).
4. Perangkat: Menggunakan dua switch/router backbone independen yang dikonfigurasi dengan protokol redundansi (seperti VRRP atau LACP).

### 8. Evaluasi Klaim Vendor: “Throughput Wi-Fi Setara PHY Rate”
Klaim vendor tersebut tidak benar. PHY rate adalah kecepatan mentah pada lapisan fisik, sedangkan throughput nyata aplikasi selalu jauh lebih rendah akibat overhead:
* Preamble dan Header (MAC & PHY Layer).
* Sinyal kontrol (Frame ACK, RTS/CTS).
* Waktu tunggu CSMA/CA (Interframe spacing & Backoff delay).
* Retransmisi paket akibat kebocoran sinyal/interferensi.

### 9. Mengapa IMT-2030 (2026) Diajarkan Sebagai Proses Standardisasi
Karena pada tahun 2026, IMT-2030 (6G) masih berada dalam fase konsolidasi visi, penelitian spektrum frekuensi, dan perumusan dokumen persyaratan teknis oleh ITU-R dan 3GPP. Pembelajaran difokuskan pada metodologi penyusunan standar internasional, bukan sebagai produk komersial yang sudah jadi.

### 10. Rancangan Praktikum Hubungan Jarak, SNR, Modulasi, dan Throughput
* Alat & Bahan: 2 unit Access Point / Router Wi-Fi, 1 laptop penguji, dan perangkat lunak iPerf.
* Prosedur:
  1. Dekatkan laptop ke AP (jarak 1 meter), catat nilai SNR, tingkat modulasi (MCS Index), dan jalankan iPerf untuk mengukur throughput.
  2. Pindahkan laptop secara

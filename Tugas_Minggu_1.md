# KONSEP JARINGAN
## Latihan Konsep Dasar Jaringan Komputer
**DOSEN:** 
Dr. Ferry Astika Saputra, ST, M.Sc. (197708232001121002)

**PENYUSUN:**
Zakil Muannisi (3125600032)

**PROGRAM STUDI SARJANA TERAPAN TEKNIK INFORMATIKA
POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**



---

## Level A — Ingatan dan Pemahaman

### 1. Pengertian Jaringan Komputer
Jaringan komputer adalah sistem yang menghubungkan dua atau lebih perangkat untuk saling bertukar data dan berbagi sumber daya.
Empat unsur pokoknya:
* Perangkat (host atau node)
* Media transmisi (kabel atau nirkabel)
* Aturan komunikasi (protokol)
* Layanan atau aplikasi yang memanfaatkan jaringan

### 2. Perangkat Otonom
Perangkat otonom adalah sistem komputer yang memiliki prosesor, memori, dan sistem operasi sendiri. Perangkat ini dapat memproses data secara mandiri tanpa harus dikendalikan penuh oleh komputer lain.

### 3. Perbedaan Data, Sinyal, dan Paket
* Data: Informasi mentah yang dipahami oleh manusia atau aplikasi (contoh: teks, gambar, video).
* Sinyal: Gelombang fisik (listrik, cahaya, atau radio) yang membawa data melintasi media transmisi.
* Paket: Potongan data yang telah diberi header (alamat asal dan tujuan) agar dapat dikirim melalui jaringan.

### 4. Perbedaan PAN, LAN, MAN, dan WAN
* PAN (Personal Area Network): Jaringan antar-perangkat pribadi dalam jarak sangat dekat (contoh: koneksi Bluetooth HP ke TWS).
* LAN (Local Area Network): Jaringan area terbatas yang dikelola sendiri (contoh: jaringan di dalam satu rumah atau gedung kantor).
* MAN (Metropolitan Area Network): Jaringan yang menghubungkan beberapa LAN dalam lingkup satu kota (contoh: jaringan antar-kampus atau kantor cabang se-kota).
* WAN (Wide Area Network): Jaringan berskala luas yang melintasi batas kota, pulau, atau negara dan mengandalkan penyedia jasa telekomunikasi.

### 5. Perbedaan Intranet, Ekstranet, dan Internet Publik
* Intranet: Jaringan privat yang hanya dapat diakses oleh internal anggota organisasi.
* Ekstranet: Jaringan privat yang sebagian fiturnya dibuka terbatas untuk pihak luar tepercaya (seperti mitra bisnis atau vendor).
* Internet Publik: Jaringan global terbuka yang dapat diakses oleh siapa saja di seluruh dunia.

### 6. Mengapa Wi-Fi Tidak Sama dengan Internet?
Wi-Fi hanyalah teknologi nirkabel lokal untuk menghubungkan perangkat ke router. Wi-Fi berperan seperti "kabel tanpa wujud". Sedangkan Internet adalah jaringan global di luar router tersebut. Kamu tetap bisa terhubung ke Wi-Fi rumah meskipun koneksi Internet sedang putus.

### 7. Perbedaan Client, Server, dan Peer
* Client: Perangkat yang meminta layanan atau data.
* Server: Perangkat yang menyediakan layanan atau data.
* Peer: Perangkat yang berkedudukan setara, bisa bertindak sebagai client dan server sekaligus.

### 8. Perbedaan Bandwidth, Throughput, dan Goodput
* Bandwidth: Kapasitas maksimal teoritis dari suatu jalur jaringan.
* Throughput: Kecepatan nyata pengiriman data yang berhasil melewati jalur tersebut dalam waktu tertentu.
* Goodput: Kecepatan bersih data berguna yang diterima oleh aplikasi (tidak dihitung header dan paket yang rusak/diulang).

### 9. Empat Komponen Nodal Delay
* Processing delay: Waktu yang dibutuhkan router untuk memeriksa header paket.
* Queuing delay: Waktu tunggu paket di dalam antrean router.
* Transmission delay: Waktu yang dibutuhkan untuk mendorong seluruh bit paket ke media transmisi.
* Propagation delay: Waktu tempuh sinyal melintasi media fisik dari pengirim ke penerima.

### 10. Mengapa Web Tidak Sama dengan Internet?
Internet adalah infrastruktur jaringannya (seperti jaringan jalan raya), sedangkan Web (World Wide Web) adalah salah satu layanan yang berjalan di atas Internet (seperti mobil yang melintas), khusus untuk mengakses dokumen dan halaman situs via browser.

---

## Level B — Penerapan dan Analisis

### 1. Perhitungan Transmission Delay
* Ukuran paket = 1.000 byte = 8.000 bit
* Kecepatan tautan = 10 Mbps = 10.000.000 bit per detik
* Transmission delay = 8.000 / 10.000.000 = 0,0008 detik (0,8 milidetik)
* Komponen delay yang belum tercakup: Processing delay, queuing delay, dan propagation delay.

### 2. Lima Hipotesis Throughput Rendah di Satu Lantai Kampus
* Sinyal Wi-Fi di lantai tersebut mengalami interferensi frekuensi atau terhalang dinding tebal.
* Kabel LAN penyambung dari switch utama ke Access Point (AP) di lantai itu mengalami kerusakan atau berkualitas rendah.
* Terjadi penumpukan pengguna (overcrowded) yang terhubung ke satu AP secara bersamaan.
* Pengaturan batas kecepatan (bandwidth throttling) yang terlalu ketat pada switch di lantai tersebut.
* Ada perangkat pengguna di lantai itu yang terinfeksi malware dan menghabiskan kapasitas jaringan.

### 3. Kebutuhan Jaringan: Transfer Berkas vs Panggilan Video
* Transfer Berkas Cadangan: Membutuhkan Throughput tinggi dan Integritas data mutlak (tidak boleh ada data hilang). Latensi dan jitter tidak terlalu berpengaruh.
* Panggilan Video: Membutuhkan Latensi rendah dan Jitter (variasi delay) yang stabil. Throughput yang dibutuhkan sedang dan toleran terhadap sedikit kehilangan data.

### 4. Evaluasi Redundansi Dua Operator
Kualitas redundansinya sangat buruk (semu). Jika tiang atau jalur ducting tersebut roboh, terbakar, atau terputus akibat galian, kedua jalur dari kedua operator akan putus secara bersamaan (single point of failure).

### 5. Mengapa Tambah Bandwidth Tidak Selalu Mempercepat Akses Jauh?
Karena waktu akses ke server yang sangat jauh lebih didominasi oleh propagation delay (jarak fisik dan batas kecepatan cahaya dalam serat optik) serta banyaknya lompatan router di perjalanan. Menambah bandwidth tidak akan mempercepat waktu tempuh fisik sinyal tersebut.

### 6. Perhitungan Ketidaktersediaan (Availability)
* Ketersediaan 99,9% per tahun (8.760 jam): Maksimum durasi mati (downtime) adalah 8,76 jam per tahun.
* Target 99,99% per tahun: Maksimum durasi mati (downtime) turun drastis menjadi hanya sekitar 52,6 menit per tahun.

### 7. Perbandingan Client-Server vs P2P untuk File Besar
* Client-Server:
  * Kelebihan: Pengelolaan terpusat dan keamanan data mudah dikontrol.
  * Kelemahan: Server rawan tumbang atau lambat (bottleneck) saat diakses ribuan pengguna bersamaan.
* P2P (Peer-to-Peer):
  * Kelebihan: Beban terbagi rata, semakin banyak yang mengunduh semakin cepat distribusi berkasnya.
  * Kelemahan: Keamanan sulit dijamin, serta sangat bergantung pada ketersediaan pengguna lain yang membagikan berkas (seeder).

### 8. Perbedaan Topologi Fisik dan Logis
Contohnya pada jaringan yang menggunakan Switch/Hub. Secara fisik, kabel dari semua komputer dicolok memusat ke satu switch (topologi Bintang/Star). Namun secara logis, aliran sinyal di dalam hub dipancarkan ke semua port (topologi Bus).

---

## Level C — Evaluasi dan Sintesis

### 1. Klasifikasi Kebutuhan Jaringan Kampus
* Mahasiswa dan Tamu: Ditempatkan di VLAN khusus dengan bandwidth dibatasi dan hanya diberi akses ke Internet publik.
* Staf Administrasi: VLAN privat dengan akses terenkripsi ke server data kampus dan internet internal.
* Kamera Pengawas (CCTV): VLAN terisolasi total tanpa akses ke Internet publik, hanya bisa mengirim data ke server rekaman.
* Laboratorium Riset: VLAN khusus dengan kapasitas bandwidth tinggi dan prioritas jalur.
* Alasan Segmentasi: Menjaga keamanan data sensitif, mengisolasi kebocoran jaringan, dan menjaga stabilitas trafik.

### 2. Evaluasi: "Jaringan internal tidak perlu enkripsi karena ada firewall"
Pernyataan tersebut salah.
* Kerahasiaan: Firewall hanya menjaga benteng luar. Jika ada ancaman dari dalam (orang dalam atau perangkat terinfeksi), data tanpa enkripsi mudah diintip.
* Integritas: Tanpa enkripsi, data dalam jaringan lokal dapat diubah di tengah jalan oleh pihak yang tidak berhak.
* Ketersediaan: Enkripsi membantu mencegah manipulasi paket yang dapat merusak layanan internal.

### 3. Mengapa Internet Bisa Berkembang Tanpa Otoritas Pusat Tunggal?
Internet berkembang menggunakan prinsip standar terbuka (open standards) yang disepakati bersama oleh komunitas global (seperti IETF) melalui dokumen terbuka (RFC).
* Manfaat: Inovasi berkembang sangat cepat, fleksibel, dan tidak dikuasai oleh satu negara atau perusahaan.
* Risiko: Kesulitan dalam penegakan hukum global, penanganan kejahatan siber, dan koordinasi keamanan skala besar.

### 4. Circuit Switching vs Packet Switching untuk Suara
* Circuit Switching: Menyediakan jalur khusus yang dijamin kecepatannya, namun sangat boros sumber daya karena jalur terkunci meskipun tidak ada percakapan.
* Packet Switching: Memotong data menjadi paket-paket kecil dan berbagi jalur dengan data lain sehingga jauh lebih efisien.
* Mengapa Suara Modern Bisa di Packet Switching: Karena kecepatan jaringan saat ini sudah sangat tinggi dan dilengkapi fitur QoS (Quality of Service) untuk memprioritaskan paket suara agar tidak patah-patah.

### 5. Prosedur Diagnosis "Internet Lambat"
1. Pengumpulan Bukti: Lakukan tes ping ke router lokal dan tes kecepatan (speedtest) ke server luar. Catat nilai throughput, latensi, dan persentase paket hilang.
2. Pengujian Hipotesis: Hubungkan komputer langsung menggunakan kabel LAN ke router utama. Jika lancar, masalah ada pada jaringan Wi-Fi lokal.
3. Kriteria Keberhasilan: Nilai ping stabil tanpa paket hilang dan kecepatan throughput mencapai minimal 80% dari paket langganan.

### 6. Statistik IPv6 Indonesia
* Metrik: Persentase tingkat adopsi IPv6 (pengguna yang mengakses internet menggunakan protokol IPv6).
* Perbedaan Angka APNIC vs Google: Terjadi karena perbedaan metodologi. Google mengukur dari sampel pengguna yang aktif mengakses layanan Google, sedangkan APNIC mengukur berdasarkan alokasi IP dan data dari ISP.

### 7. Satelit Orbit Rendah (LEO) untuk Kampus Terpencil
* Kinerja: Latensi jauh lebih rendah dibanding satelit generasi lama, sangat memadai untuk panggilan video dan kuliah online.
* Biaya: Biaya berlangganan dan perangkat lebih mahal dibanding kabel lokal, tetapi jauh lebih murah dibanding menarik serat optik ke area terpencil.
* Ketergantungan Cuaca: Performa sinyal dapat menurun saat terjadi hujan sangat lebat.
* Keamanan dan Pengelolaan: Pengelolaan dari sisi kampus relatif praktis, tetapi sangat bergantung pada penyedia layanan satelit.

### 8. Otomatisasi Jaringan: Dampak dan Kontrol Risiko
* Sisi Positif: Menghilangkan kesalahan pengetikan manual dan mempercepat proses konfigurasi ratusan perangkat.
* Risiko: Kesalahan kecil pada skrip otomatisasi akan berdampak fatal secara massal ke seluruh jaringan dalam hitungan detik.
* Kontrol Risiko: Lakukan pengujian skrip di lingkungan simulasi sebelum diterapkan, sediakan fitur pembatalan otomatis (rollback) jika terjadi kegagalan, serta terapkan perubahan secara bertahap (canary deployment).
Tugas ini dibuat untuk memenuhi modul jaringan komputer.

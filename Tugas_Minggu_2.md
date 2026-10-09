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

### 1. Alasan Komunikasi Jaringan Disusun Berlapis
* Modularitas: Memecah sistem kompleks menjadi komponen yang lebih mudah dirancang dan dikelola.
* Interoperabilitas: Memungkinkan perangkat dari vendor berbeda saling berkomunikasi sesuai standar.
* Fleksibilitas: Perubahan teknologi di satu lapisan tidak merusak fungsi di lapisan lain.

### 2. Perbedaan Layanan, Antarmuka, dan Protokol
* Layanan: Fungsi yang disediakan oleh suatu lapisan untuk lapisan di atasnya.
* Antarmuka: Titik akses (SAP) bagi lapisan atas untuk meminta layanan lapisan bawah.
* Protokol: Aturan dan format komunikasi antar-entitas setara di lapisan yang sama pada perangkat berbeda.

### 3. Tujuh Lapisan OSI (Bawah ke Atas)
1. Physical: Pengiriman bit data mentah melalui media fisik.
2. Data Link: Pembentukan frame, kontrol akses media (MAC), dan deteksi error.
3. Network: Pengalamatan logis (IP) dan pengarahan rute (routing).
4. Transport: Komunikasi end-to-end, kontrol aliran, dan segmentasi data.
5. Session: Membuka, mengelola, dan menghentikan sesi komunikasi.
6. Presentation: Enkripsi, kompresi, dan penerjemahan format data.
7. Application: Antarmuka langsung bagi aplikasi pengguna.

### 4. Empat Lapisan Model TCP/IP
1. Application Layer
2. Transport Layer
3. Internet Layer
4. Network Access Layer

### 5. Alasan Model TCP/IP Kadang Disajikan 5 Lapisan
Model 5 lapisan memisahkan Network Access Layer menjadi lapisan Physical dan Data Link agar mempermudah analisis perangkat keras dan pengalamatan fisik untuk tujuan pembelajaran.

### 6. Perbedaan Frame, IP Packet, TCP Segment, dan UDP Datagram
* TCP Segment: PDU Transport Layer yang connection-oriented dan terjamin keandalannya.
* UDP Datagram: PDU Transport Layer yang connectionless dan tanpa jaminan pengiriman.
* IP Packet: PDU Network Layer yang membungkus segment/datagram dengan IP asal dan tujuan.
* Frame: PDU Data Link Layer yang membungkus IP packet dengan MAC address asal dan tujuan.

### 7. Definisi Header, Trailer, dan Payload
* Header: Informasi kontrol di awal unit data (IP, port, sequence number).
* Payload: Data asli yang dibawa oleh PDU dari lapisan atasnya.
* Trailer: Informasi kontrol di akhir unit data untuk pemeriksaan error (FCS/CRC).

### 8. Enkapsulasi dan Dekapsulasi
* Enkapsulasi: Proses pembungkusan data dengan header/trailer dari lapisan atas ke bawah di perangkat pengirim.
* Dekapsulasi: Proses pelepasan header/trailer dari lapisan bawah ke atas di perangkat penerima.

### 9. Fungsi Multiplexing dan Demultiplexing
* Multiplexing: Menggabungkan trafik dari berbagai aplikasi pengirim ke satu jalur jaringan (menggunakan port).
* Demultiplexing: Memisahkan trafik penerima ke aplikasi yang tepat berdasarkan informasi header (port tujuan).

### 10. Mengapa OSI Bukan Spesifikasi Implementasi?
Model OSI adalah kerangka acuan teoritis yang mendefinisikan apa yang harus dilakukan tiap lapisan, bukan protokol software nyata yang diimplementasikan secara langsung di sistem operasi.

---

## Level B — Penerapan dan Analisis

### 1. Pemetaan Protokol ke Model TCP/IP
* Application: HTTP, DNS
* Transport: TCP, UDP, TLS (bekerja di atas TCP sebelum aplikasi), QUIC (berjalan di atas UDP tetapi menyediakan fungsi transport ke atas)
* Internet: IPv6, ICMP (protokol bantu IP)
* Network Access: Ethernet, Wi-Fi

### 2. Enkapsulasi DNS
* Skema: Ethernet Header | IPv4 Header | UDP Header | DNS Data | Ethernet Trailer
* Pengenal tiap batas:
  * Ethernet: MAC Address Asal & Tujuan, EtherType (0x0800)
  * IPv4: IP Address Asal & Tujuan, Protocol Number (17 untuk UDP)
  * UDP: Port Asal & Port Tujuan (53)
  * DNS: Transaction ID

### 3. Enkapsulasi HTTP/3 melalui QUIC
* Skema: Ethernet Header | IP Header | UDP Header | QUIC Header | HTTP/3 Data | Ethernet Trailer
* Pengenal tiap batas: MAC Address (Ethernet), IP Address (IP), Port 443 (UDP), Connection ID / CID (QUIC).
* Kedudukan QUIC: QUIC menggunakan UDP hanya sebagai pembungkus transmisi, sedangkan fungsi kontrol keandalan, urutan, multiplexing, dan enkripsi ditangani sendiri oleh QUIC.

### 4. Perubahan Header Saat Melewati Router (Tanpa NAT)
* Berubah: MAC Address Asal & Tujuan (Data Link Header), serta TTL dan Checksum pada IP Header.
* Tetap: IP Address Asal & Tujuan, Port Asal & Tujuan, serta Payload.

### 5. Perubahan Jika Router Melakukan NAT/PAT
* IP Address Asal atau Tujuan diubah ke IP Publik router.
* Port Asal atau Tujuan pada header TCP/UDP ikut diubah dan checksum dihitung ulang.

### 6. Hipotesis Checksum TCP Salah pada Packet Capture
Terjadi akibat fitur TCP Checksum Offload (Tx Offload) pada NIC. Aplikasi capture menangkap paket sebelum diproses oleh hardware NIC, sehingga checksum masih berupa nilai sementara.

### 7. Diagnosis Pengguna Buka IP Bisa, Buka Nama Gagal
* Application: Server DNS mati atau nama domain salah.
* Transport: Port UDP/TCP 53 diblokir firewall.
* Internet: Rute ke server DNS terputus.
* OS/Host: Pemetaan salah pada file hosts lokal.

### 8. Enam Hipotesis Ping Berhasil tapi HTTPS Gagal
1. Port TCP 443 diblokir firewall.
2. Server web mati (layanan HTTPS tidak listening).
3. Gagal TLS Handshake akibat sertifikat kadaluarsa atau cipher suite mismatch.
4. Terjadi masalah Path MTU Discovery (paket TLS besar terbuang).
5. Antrean TCP SYN backlog pada server penuh.
6. Mismatch pada header Server Name Indication (SNI).

### 9. Sesi Aplikasi vs Koneksi TCP
* Koneksi TCP: Terikat pada IP dan Port. Jika IP berubah, koneksi putus.
* Sesi Aplikasi: Menjaga status pengguna terlepas dari koneksi jaringan.
* Contoh: Pengguna pindah dari Wi-Fi ke 4G saat streaming video; sesi login tetap aktif karena menggunakan QUIC Connection ID atau session token.

### 10. Mengapa Enkripsi Tidak Selalu di Presentation Layer?
Enkripsi dibutuhkan di berbagai lapisan sesuai kebutuhan keamanan (defense in depth): MACsec di Data Link mengamankan kabel fisik, IPsec di Network mengamankan tunnel antar-site, dan TLS di Transport mengamankan aplikasi spesifik.

---

## Level C — Evaluasi dan Sintesis

### 1. Evaluasi Pernyataan Model OSI
Pernyataan tersebut keliru. Meski protokol OSI tidak dipakai di internet, model OSI tetap sangat relevan secara pedagogis dan sebagai bahasa standar industri untuk mengkategorikan fungsi jaringan (seperti sebutan Layer 2 Switch atau Layer 7 Firewall).

### 2. Analisis Strict Layering
* Keuntungan: Penanganan masalah terisolasi dan pengembang dapat mengubah satu lapisan tanpa merusak lapisan lain.
* Kerugian: Overhead enkapsulasi dan potensi penurunan performa karena lapisan atas tidak tahu kondisi fisik jaringan.
* Cross-layer: Membantu pada jaringan nirkabel (memberitahu statistik sinyal ke TCP), tetapi merusak modularitas jika aplikasi dibuat bergantung pada hardware fisik tertentu.

### 3. Penelusuran Gangguan Wi-Fi Jam Sibuk
* Physical (L1): RSSI/SNR buruk akibat interferensi frekuensi radio.
* Data Link (L2): High frame drop dan delay akibat kepadatan medium nirkabel (CSMA/CA).
* Network (L3): Latensi tinggi (jitter) akibat antrean bufferbloat di router/AP.
* Transport (L4): Packet loss pada paket UDP/RTP yang menyebabkan video patah-patah.

### 4. Skenario Lab Perekaman Paket Tanpa Data Sensitif
Akses halaman API publik (contoh: api.github.com) menggunakan curl atau incognito browser. Filter capture hanya pada IP tujuan. Protokol yang tertangkap mencakup Ethernet, IP, TCP handshake, TLS handshake, dan frame HTTP/2 tanpa menyimpan cookie atau kredensial pribadi.

### 5. Urutan Header VXLAN di Atas IPsec & Risiko MTU
* Urutan: Outer Eth | Outer IP (IPsec) | ESP | Inner IP | UDP | VXLAN | Inner Eth | Inner IP | TCP/UDP | Data
* Risiko MTU: Enkapsulasi ganda menambah overhead >100 byte. Jika MTU tetap 1500 byte, akan terjadi fragmentasi berat yang menurunkan performa. Solusi: Gunakan Jumbo Frame (MTU 9000).

### 6. Perbandingan Enkripsi
* MACsec (L2): Titik terminasi port fisik switch. Mencakup keamanan link lokal.
* IPsec (L3): Titik terminasi router/gateway. Mencakup seluruh lalu lintas antar-network.
* TLS (L4/L7): Titik terminasi antar-proses aplikasi. Mencakup keamanan data aplikasi.
* App E2EE (L7): Titik terminasi perangkat pengguna akhir. Server perantara tidak dapat membaca data.

### 7. Tantangan Peralatan Jaringan Modern
Firewall NGFW, Proxy, dan Load Balancer melanggar batas lapisan (layer violation) dengan membaca hingga payload L7 untuk melakukan inspeksi keamanan, pemutusan koneksi, dan pembagian beban trafik.

### 8. Evaluasi Integritas Berkas (End-to-End Principle)
Pemeriksaan integritas paling tepat diletakkan di Application Layer (misal menggunakan HASH SHA-256). Pemeriksaan di router atau transport layer tidak menjamin berkas terhindar dari kerusakan saat disimpan di penyimpanan atau diproses oleh aplikasi akhir.

### 9. Analisis Retry Storm
Terjadi ketika Client (L7), Proxy/Mesh (L7), dan TCP (L4) melakukan retransmisi secara bersamaan saat terjadi latensi tinggi. Jumlah request membengkak secara eksponensial dan memicu kegagalan sistem secara massal (cascading failure).

### 10. Argumen Model Terbaik untuk Pemula
Model 5 Lapisan (Physical, Data Link, Network, Transport, Application) adalah yang terbaik.
* Tujuan: Memberikan pemahaman teori yang presisi dan sesuai dengan realitas internet.
* Manfaat: Memisahkan L1 dan L2 agar perbedaan kabel/MAC jelas, serta menghilangkan kerumitan Session/Presentation OSI yang jarang berupa modul terpisah pada software modern.
* Keterbatasan: Fungsi enkripsi/kompresi harus dijelaskan sebagai bagian dari Application layer.

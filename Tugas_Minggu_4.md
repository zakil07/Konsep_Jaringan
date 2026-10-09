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

### 1. Kedudukan Data Link Layer
Data Link Layer berada di antara Physical Layer (Layer 1) dan Network Layer (Layer 3).
* Ke bawah: Mengubah bit mentah dari Physical Layer menjadi bingkai data (frame) yang terstruktur.
* Ke atas: Menyediakan pengalamatan fisik (MAC Address) dan transmisi data bebas kesalahan agar paket dari Network Layer dapat melintasi media fisik lokal.

### 2. Fungsi Utama LLC dan MAC
* Logical Link Control (LLC): Berinteraksi dengan Network Layer di atasnya, mengidentifikasi protokol jaringan (misal IPv4/IPv6), dan mengelola kontrol aliran data.
* Medium Access Control (MAC): Berinteraksi langsung dengan Physical Layer, mengelola pengalamatan fisik (MAC Address), dan mengatur hak akses perangkat ke media transmisi.

### 3. Tujuan Framing
Mengelompokkan deretan bit mentah dari Physical Layer menjadi unit data terstruktur (frame) yang memiliki batas awal dan akhir yang jelas, sehingga penerima dapat mengenali awal paket, alamat tujuan, dan memeriksa kesalahan data.

### 4. Perbedaan Parity, Checksum, CRC, dan FEC
* Parity: Metode pendeteksi kesalahan sederhana dengan menambahkan 1 bit penanda (genap/ganjil) pada sekelompok bit.
* Checksum: Pendeteksi kesalahan dengan menjumlahkan nilai byte data dan menyimpan hasil penjumlahannya pada header.
* CRC (Cyclic Redundancy Check): Pendeteksi kesalahan berbasis pembagian polinomial matematis yang sangat akurat untuk mendeteksi error pada rantai bit.
* FEC (Forward Error Correction): Metode yang tidak hanya mendeteksi error, tetapi juga dapat memperbaiki bit yang rusak di sisi penerima tanpa perlu meminta pengiriman ulang.

### 5. Field Utama Frame Ethernet II
* Preamble & SFD: Penyelaras sinyal clock dan penanda awal frame.
* Destination MAC: Alamat fisik perangkat penerima (6 byte).
* Source MAC: Alamat fisik perangkat pengirim (6 byte).
* Type/EtherType: Jenis protokol lapisan atas (misal 0x0800 untuk IPv4).
* Payload: Data asli dari lapisan atas (46 - 1500 byte).
* FCS (Frame Check Sequence): Kode pemeriksaan kesalahan CRC (4 byte).

### 6. Perbedaan Unicast, Multicast, dan Broadcast MAC
* Unicast MAC: Alamat tujuan khusus untuk satu perangkat tertentu (contoh: `00:1A:2B:3C:4D:5E`).
* Multicast MAC: Alamat tujuan untuk sekelompok perangkat tertentu yang mendaftar (diawali `01:00:5E` untuk IPv4).
* Broadcast MAC: Alamat tujuan untuk seluruh perangkat dalam satu jaringan lokal (`FF:FF:FF:FF:FF:FF`).

### 7. Fungsi Learning, Forwarding, Filtering, dan Flooding pada Switch
* Learning: Mencatat MAC Address asal dan nomor port masuknya ke dalam MAC Address Table.
* Forwarding: Meneruskan frame langsung ke port tujuan tertentu jika MAC tujuannya sudah terdaftar di tabel.
* Filtering: Membuang atau tidak meneruskan frame ke port lain jika MAC tujuan berada pada segment port yang sama dengan pengirim.
* Flooding: Mengirimkan frame ke seluruh port (kecuali port asal) jika MAC tujuan belum ada di tabel atau berupa broadcast/multicast.

### 8. Perbedaan Collision Domain dan Broadcast Domain
* Collision Domain: Area jaringan tempat sinyal data dapat bertabrakan jika dua perangkat mengirimkan data bersamaan (dipisahkan oleh Switch/Bridge).
* Broadcast Domain: Area jaringan tempat lalu lintas broadcast akan diteruskan dan diterima oleh semua perangkat (dipisahkan oleh Router/VLAN).

### 9. Fungsi ARP dan Mencari MAC Gateway
* Fungsi ARP (Address Resolution Protocol): Menerjemahkan/memetakan IP Address yang diketahui menjadi MAC Address.
* Kapan Mencari MAC Gateway: Host mencari MAC Address gateway saat akan mengirimkan data ke tujuan yang berada pada subnet atau jaringan yang berbeda.

### 10. Mengapa IPv6 Tidak Menggunakan ARP?
Karena IPv6 telah menggantikan fungsi ARP dengan protokol **NDP (Neighbor Discovery Protocol)** yang berjalan di atas ICMPv6. NDP lebih efisien karena memanfaatkan pesan multicast, bukan broadcast yang mengganggu seluruh jaringan.

### 11. Fungsi Access Port dan Trunk
* Access Port: Port switch yang hanya terhubung ke satu VLAN khusus dan membawa frame biasa tanpa tag VLAN (untagged).
* Trunk Port: Port switch yang menghubungkan antar-switch atau ke router untuk membawa trafik dari banyak VLAN sekaligus menggunakan tag IEEE 802.1Q.

### 12. Mengapa Komunikasi Antar-VLAN Memerlukan Layer 3?
Karena setiap VLAN membentuk broadcast domain dan subnet IP yang terisolasi secara logis pada Layer 2. Switch Layer 2 tidak memiliki kemampuan mengarahkan rute (routing) antar-subnet, sehingga dibutuhkan fungsi Layer 3 (Router atau Switch Layer 3).

---

## Level B — Penerapan dan Analisis

### 1. ARP dan Frame Data pada Subnet Sama (Host A ke B)
1. **ARP Request:** Host A mengirim broadcast (`FF:FF:FF:FF:FF:FF`) menanyakan "Siapa pemilik IP B? Beritahu A".
2. **ARP Reply:** Host B membalas secara unicast ke Host A "IP B adalah milik MAC B".
3. **Frame Data:** Host A menyimpan MAC B di tabel ARP, lalu mengirimkan frame data unicast langsung dari Source MAC A ke Destination MAC B.

### 2. ARP pada Subnet Berbeda (Host A ke B via Gateway)
* Host A menyadari IP B berada di subnet berbeda.
* Host A **mencari MAC Address milik Gateway (Router)**, bukan MAC B.
* Frame data dikirimkan dari Host A dengan Destination MAC = MAC Gateway dan Destination IP = IP B.

### 3. Proses Switch Menerima Frame dari MAC A ke B (B Belum Dikenal)
1. **Perubahan Tabel:** Switch mencatat MAC A dan nomor Port 1 ke dalam MAC Address Table.
2. **Port Keluaran:** Karena MAC B belum ada di tabel, switch melakukan **flooding** dengan meneruskan frame tersebut ke seluruh port aktif kecuali Port 1.

### 4. Akibat Dua Trunk Paralel Tanpa STP atau LAG
Akan terjadi **Bridging Loop** dan **Broadcast Storm**. Frame broadcast akan terus berputar secara tiada henti di antara kedua switch, memenuhi kapasitas bandwidth, membingungkan MAC Address Table (MAC flapping), dan membuat jaringan lumpuh (*crash*).

### 5. Ukuran Frame Ethernet Tag VLAN (Payload 1.500 Byte)
* Destination MAC (6) + Source MAC (6) + 802.1Q VLAN Tag (4) + EtherType (2) + Payload (1500) + FCS (4) = **1.522 Byte**.

### 6. Diagnosis Hilangnya VLAN 20 di Switch B
1. **Access Port:** Pastikan port ke perangkat akhir sudah dimasukkan ke VLAN 20 (`switchport access vlan 20`).
2. **Trunk Port:** Pastikan port antar-switch berstatus `trunk`.
3. **Allowed VLAN:** Periksa apakah VLAN 20 terdaftar pada konfigurasi trunk (`switchport trunk allowed vlan`).
4. **STP State:** Pastikan Spanning Tree tidak memblokir VLAN 20 di port trunk (`show spanning-tree vlan 20`).
5. **Tabel MAC:** Cek apakah MAC address perangkat muncul di tabel MAC VLAN 20 (`show mac address-table vlan 20`).

### 7. Dampak Native VLAN Mismatch
* Terjadi kebocoran trafik (*VLAN Leaking*) di mana data dari satu VLAN dapat masuk ke VLAN lain tanpa router.
* Switch akan memunculkan pesan kesalahan (*CDP/PVST error mismatch log*).
* Spanning Tree Protocol (STP) dapat memblokir port trunk untuk mencegah loop.

### 8. Mengapa LACP 4x10 Gbps Tetap 10 Gbps untuk Satu Sesi TCP?
Karena mekanis pengimbangan beban (load balancing) LACP menggunakan algoritma hash berbasis atribut paket (seperti kombinasi Source/Destination IP dan Port). Seluruh paket dalam satu sesi TCP yang sama akan menghasilkan nilai hash yang identik, sehingga selalu dilewatkan pada satu jalur fisik yang sama untuk mencegah paket sampai berurutan tidak teratur (*out-of-order*).

### 9. Perbandingan FCS dengan MACsec
* FCS (Frame Check Sequence): Hanya mendeteksi kerusakan data yang tidak disengaja (akibat noise/gangguan sinyal). FCS **tidak dapat mencegah** manipulasi data sengaja atau pembacaan rahasia.
* MACsec (IEEE 802.1AE): Menyediakan enkripsi, autentikasi, dan perlindungan integritas pada Layer 2. MACsec **mencegah** pengintipan data (*eavesdropping*), manipulasi paket, dan serangan Man-in-the-Middle pada kabel fisik.

### 10. Diagnosis Layer 2 Wi-Fi Lambat Meski RSSI Tinggi
* **Airtime Saturation:** Terlalu banyak perangkat aktif mengantre kapasitas frekuensi nirkabel yang sama.
* **High Retry Rate:** Banyak paket rusak dan diulang akibat interferensi lokal atau perangkat klien yang mentransmisikan daya terlalu kecil.
* **Contention (CSMA/CA):** Waktu tunggu (*backoff time*) tinggi akibat perebutan media transmisi oleh banyak pengguna.
* **Basic Rate Rendah:** Access Point dipaksa melayani perangkat lama (*legacy*) dengan laju data dasar yang sangat lambat, sehingga menghabiskan waktu pemancaran (*airtime*).

---

## Level C — Evaluasi dan Sintesis

### 1. Rancangan VLAN Kampus dan Aturan Inter-VLAN
* **Rancangan VLAN:** VLAN 10 (Mahasiswa), VLAN 20 (Staf/Admin), VLAN 30 (Laboratorium), VLAN 40 (IoT), VLAN 50 (Server), VLAN 60 (Management), VLAN 70 (Voice), VLAN 80 (Guest).
* **Aturan Inter-VLAN:**
  * Mahasiswa & Guest: Hanya diizinkan routing ke Internet publik dan Server

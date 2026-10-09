# KONSEP JARINGAN
## Latihan Konsep Dasar Jaringan Komputer
**DOSEN:** 
Dr. Ferry Astika Saputra, ST, M.Sc. (197708232001121002)

**PENYUSUN:**
Zakil Muannisi (3125600032)

**PROGRAM STUDI SARJANA TERAPAN TEKNIK INFORMATIKA**

**POLITEKNIK ELEKTRONIKA NEGERI SURABAYA**



---

## Level A — Pemahaman Dasar

### 1. Perbedaan Parent Prefix, Child Prefix, dan Subnet
* Parent Prefix: Blok alamat IP besar yang didelegasikan atau menjadi induk alokasi utama.
* Child Prefix: Blok alamat IP yang merupakan hasil pemecahan (sub-alokasi) dari parent prefix.
* Subnet: Jaringan logis hasil pembagian alamat IP yang membatasi domain broadcast secara spesifik.

### 2. Konversi Prefix ke Netmask Desimal
* /19 = `255.255.224.0`
* /21 = `255.255.248.0`
* /27 = `255.255.255.224`
* /29 = `255.255.255.248`

### 3. Analisis Prefix /23
* Bit Host ($h$): $32 - 23 = 9$ bit.
* Jumlah Total Alamat: $2^9 = 512$ alamat.
* Host Konvensional (Usable): $2^9 - 2 = 510$ host.

### 4. Mengapa Network ID Harus Berada pada Batas Ukuran Blok?
Karena operasi bitwise AND antara IP address dengan subnet mask secara matematis mewajibkan seluruh bit host bernilai 0. Nilai oktet tempat pemotongan prefix harus merupakan kelipatan bulat dari ukuran blok (*block size*) terkait.

### 5. Ukuran Blok Oktet Keempat
* /26: $256 - 192 = 64$
* /27: $256 - 224 = 32$
* /28: $256 - 240 = 16$

### 6. Analisis 192.168.5.173/27
* Ukuran Blok: $256 - 224 = 32$
* Kelipatan Blok Oktet Keempat: 0, 32, 64, 96, 128, **160**, 192...
* Network ID: `192.168.5.160`
* Broadcast Address: `192.168.5.191`
* Rentang Host Usable: `192.168.5.161` sampai `192.168.5.190`

### 7. Analisis 172.16.77.9/20
* Prefix /20 mengubah oktet ketiga. Ukuran blok oktet ketiga: $2^{24-20} = 16$.
* Kelipatan 16 di oktet ketiga: 0, 16, 32, 48, **64**, 80...
* Network ID: `172.16.64.0`
* Broadcast Address: `172.16.79.255`

### 8. Pengecualian Rumus $2^h - 2$ pada /31
Berdasarkan RFC 3021, prefix /31 ditujukan khusus untuk tautan point-to-point. Kedua alamat yang ada ($2^1 = 2$) digunakan seluruhnya sebagai host IP di masing-masing ujung tautan tanpa mengalokasikan Network ID dan Broadcast Address khusus.

### 9. Fungsi /32 dalam Routing IPv4
Digunakan untuk mengidentifikasi alamat host tunggal secara presisi (*host route*), seperti alamat interface Loopback router, tujuannya untuk keperluan manajemen, ID router (OSPF/BGP), dan terminasi VPN/Tunnel.

### 10. Perbedaan Bandwidth DHCP Pool vs Kapasitas Host Subnet
* Kapasitas Host Subnet: Batas fisik/matematis jumlah IP yang valid dalam satu subnet.
* Bandwidth DHCP Pool: Batas rentang IP spesifik yang dikonfigurasi untuk dibagikan secara otomatis kepada klien (sering kali lebih kecil dari kapasitas subnet karena memisahkan IP statis).

### 11. Mengapa VLAN Tidak Sama dengan Subnet?
* VLAN: Fitur pemisahan domain broadcast secara logis pada **Layer 2** (Data Link).
* Subnet: Pemisahan ruang pengalamatan dan domain routing secara logis pada **Layer 3** (Network). Sering dipasangkan $1:1$, namun keduanya adalah konsep lapisan yang berbeda.

### 12. Tiga Blok Alamat Privat RFC 1918
* `10.0.0.0/8`
* `172.16.0.0/12`
* `192.168.0.0/16`

---

## Level B — Penerapan

### 1. Pembagian FLSM 192.168.100.0/24 Menjadi 8 Subnet (/27)

| Subnet # | Network ID | Subnet Mask | Rentang Host Usable | Broadcast Address |
| :--- | :--- | :--- | :--- | :--- |
| 1 | `192.168.100.0/27` | `255.255.255.224` | `192.168.100.1 - 192.168.100.30` | `192.168.100.31` |
| 2 | `192.168.100.32/27` | `255.255.255.224` | `192.168.100.33 - 192.168.100.62` | `192.168.100.63` |
| 3 | `192.168.100.64/27` | `255.255.255.224` | `192.168.100.65 - 192.168.100.94` | `192.168.100.95` |
| 4 | `192.168.100.96/27` | `255.255.255.224` | `192.168.100.97 - 192.168.100.126` | `192.168.100.127` |
| 5 | `192.168.100.128/27` | `255.255.255.224` | `192.168.100.129 - 192.168.100.158` | `192.168.100.159` |
| 6 | `192.168.100.160/27` | `255.255.255.224` | `192.168.100.161 - 192.168.100.190` | `192.168.100.191` |
| 7 | `192.168.100.192/27` | `255.255.255.224` | `192.168.100.193 - 192.168.100.222` | `192.168.100.223` |
| 8 | `192.168.100.224/27` | `255.255.255.224` | `192.168.100.225 - 192.168.100.254` | `192.168.100.255` |

### 2. Pembagian FLSM 172.20.0.0/16 Min 20 Subnet
* Pinjam bit: $2^5 = 32$ subnet (mencukupi min 20).
* Child Prefix: $16 + 5 =$ **/21**.
* Kapasitas Host: $2^{11} - 2 = 2.046$ usable host per subnet.

### 3. Perhitungan Kebutuhan IP VLAN (210 Host + 15 Reservasi + 20% Cadangan)
* Total Kebutuhan Dasar: $210 + 15 = 225$.
* Tambahan Cadangan 20%: $225 \times 1,2 = 270$ host.
* Ditambah Network ID & Broadcast: $270 + 2 = 272$ IP.
* Prefix yang Layak: **`/23`** (Kapasitas 512 IP / 510 usable host). Asumsi pembulatan ke atas menggunakan kelipatan $2^n$ terdekat ($2^9 = 512$).

### 4. Evaluasi Alignment 10.1.8.0/21
* Ukuran blok /21 pada oktet ketiga: $2^{24-21} = 8$.
* Kelipatan 8 oktet ketiga: 0, **8**, 16...
* Kesimpulan: **ALIGNED** (Network ID yang benar adalah `10.1.8.0/21`).

### 5. Bukti Overlap 10.1.16.0/20 dan 10.1.24.0/21
* Interval Alamat `10.1.16.0/20`: `10.1.16.0` sampai `10.1.31.255`.
* Interval Alamat `10.1.24.0/21`: `10.1.24.0` sampai `10.1.31.255`.
* Bukti: Rentang alamat `10.1.24.0/21` berada sepenuhnya di dalam interval `10.1.16.0/20`. Keduanya **OVERLAP**.

### 6. Perancangan VLSM 192.168.50.0/24
1. **Butuh 110 host:** Alokasikan `/25` $\rightarrow$ Network `192.168.50.0/25` (Rentang: `.1 - .126`).
2. **Butuh 50 host:** Alokasikan `/26` $\rightarrow$ Network `192.168.50.128/26` (Rentang: `.129 - .190`).
3. **Butuh 20 host:** Alokasikan `/27` $\rightarrow$ Network `192.168.50.192/27` (Rentang: `.193 - .222`).
4. **Butuh 10 host:** Alokasikan `/28` $\rightarrow$ Network `192.168.50.224/28` (Rentang: `.225 - .238`).
5. **Butuh 2 host:** Alokasikan `/30` $\rightarrow$ Network `192.168.50.240/30` (Rentang: `.241 - .242`).

### 7. Perbandingan /30 dan /31 pada 200 Link Point-to-Point
* Penggunaan /30: Membutuhkan $200 \times 4 = 800$ IP.
* Penggunaan /31: Membutuhkan $200 \times 2 = 400$ IP.
* Alamat Dihemat: **400 IP** (Penghematan 50%).

### 8. Pembagian DHCP Pool 10.10.8.0/23 (Total 512 IP: 10.10.8.0 - 10.10.9.255)
* Gateway: `10.10.8.1`
* Alamat Infrastruktur/Statis (20 IP): `10.10.8.1` - `10.10.8.20`
* Cadangan 15% ($\approx 76$ IP): `10.10.9.180` - `10.10.9.255`
* Dynamic DHCP Pool: `10.10.8.21` - `10.10.9.179`

### 9. Ringkasan (Agregasi) 10.40.8.0/24 - 10.40.11.0/24
* Blok oktet ketiga: 8, 9, 10, 11 (total 4 blok).
* Biner oktet ketiga: `00001000`, `00001001`, `00001010`, `00001011`.
* Bit yang sama: 6 bit pertama (`000010XX`).
* Hasil Agregasi: **`10.40.8.0/22`**.
* Syarat Alignment: Jumlah subnet harus kelipatan $2^n$ dan diawali pada batas kelipatan ukuran blok (8 adalah kelipatan 4).

### 10. Alasan 10.40.9.0/24 hingga 10.40.12.0/24 Tidak Bisa Menjadi /22
Karena blok tidak diawali dari kelipatan ukuran blok /22 (yaitu kelipatan 4: 0, 4, 8, 12, dst.). Oktet 9 bukan batas alignment untuk /22, serta rentang 9, 10, 11, 12 tidak membentuk blok biner yang contiguous.

### 11. Jumlah Subnet /64 dari IPv6 /56
* Selisih prefix: $64 - 56 = 8$ bit.
* Jumlah Subnet: $2^8 = \mathbf{256}$ **subnet /64**.

### 12. Enam Prefix /64 Pertama dari 2001:db8:abcd:1200::/56
1. `2001:db8:abcd:1200::/64`
2. `2001:db8:abcd:1201::/64`
3. `2001:db8:abcd:1202::/64`
4. `2001:db8:abcd:1203::/64`
5. `2001:db8:abcd:1204::/64`
6. `2001:db8:abcd:1205::/64`

### 13. Fungsi /64, /127, dan /128 pada IPv6
* /64: Ukuran standar subnet IPv6 untuk segmen LAN (wajib agar SLAAC berfungsi).
* /127: Digunakan khusus untuk link point-to-point antar-router (RFC 6164).
* /128: Alamat tunggal (*host route*), digunakan untuk Loopback interface.

---

## Level C — Analisis dan Evaluasi

### 1. Strategi Renumbering Bertahap Kampus 10.0.0.0/8
1. **Audit & Desain:** Buat hierarki baru (misal `10.[Gedung].[Fungsi].[Host]/24`).
2. **Abstraksi L3:** Konfigurasi dual-IP/secondary IP pada gateway router.
3. **Migrasi DHCP:** Ubah opsi scope DHCP per gedung secara bertahap menggunakan lease time pendek.
4. **Update DNS & Firewall:** Perbarui record internal dan ACL secara sistematis.
5. **Pembersihan:** Hapus IP lama dan lakukan summarization rute di router Core per gedung.

### 2. Merger Perusahaan dengan Alamat Overlap 10.10.0.0/16
* Jangka Pendek: Gunakan **Bi-directional NAT / Twice-NAT** pada border router interconnect untuk mentranslasikan IP kedua belah pihak. *Risiko:* Menyulitkan inspeksi, aplikasi kompleks (Voice/P2P) bisa rusak.
* Jangka Panjang: Lakukan **Renumbering** pada salah satu perusahaan ke blok IP privat lain (`10.X.0.0` atau `172.16.0.0/12`). *Risiko:* Downtime layanan dan beban operasional tinggi.

### 3. Evaluasi Alokasi /20 untuk 30 Perangkat IoT
Rancangan tersebut **SANGAT BURUK**.
* Pemborosan ruang alamat secara ekstrem ($4.094$ IP terbuang untuk 30 host).
* Memperluas domain broadcast secara tidak perlu yang berpotensi menurunkan kinerja perangkat IoT berdaya rendah.
* Solusi: Gunakan /27 atau /26 (disertai cadangan) dan pisahkan dalam VLAN terisolasi.

### 4. Risiko Summary /20 Tanpa Subnet Lengkap & Peran Null Route
* Risiko: Jika ada paket menuju IP dalam rentang /20 yang belum dipasang subnetnya, router dapat melempar paket ke default route, memicu **Routing Loop** antar-router hingga TTL habis.
* Peran Null Route: Memasang rute statis `ip route 10.0.0.0 255.255.240.0 Null0` di router aggregator. Paket menuju child subnet yang tidak eksis akan dibuang (*drop*) secara lokal untuk mencegah loop.

### 5. Rancangan Skema Dual-Stack (Parent IPv4 /16 & IPv6 /48)

* **Agregat Gedung A:** IPv4 `10.100.0.0/18` | IPv6 `2001:db8:1111:1000::/52`
* **Agregat Gedung B:** IPv4 `10.100.64.0/18` | IPv6 `2001:db8:1111:2000::/52`
* **Agregat Gedung C:** IPv4 `10.100.128.0/18` | IPv6 `2001:db8:1111:3000::/52`
* **Agregat Gedung D:** IPv4 `10.100.192.0/18` | IPv6 `2001:db8:1111:4000::/52`

### 6. Daftar Kontrol Validasi Otomasi IPAM
* Check overlap interval IP dengan subnet yang sudah ada.
* Check alignment batas bit prefix.
* Validasi keberadaan gateway dan ketersediaan IP pool min 15%.
* Dry-run sintaks konfigurasi pada router/DHCP server.
* Verifikasi konsistensi entitas Reverse DNS (PTR record).

### 7. Prediksi Gejala Host A (/24) vs Host B & Gateway (/26)
* Gejala: Host A dapat mengirim paket ke B jika IP B ada di rentang `.1 - .62`. Namun jika IP B berada di atas `.64`, Host A menganggap B lokal, sedangkan B menganggap A berada di luar subnet dan membutuhkan gateway.
* Paket ARP: Host B akan mengirim ARP Request ke Gateway untuk mencari A, sementara Host A mengirim ARP Request langsung mencari MAC Host B. Terjadi kegagalan komunikasi asimetris.

### 8. Evaluasi Pemberian IPv6 /48 ke Setiap End Site
**Tepat dan Sangat Direkomendasikan (Standar RFC/APNIC).**
* Sesuai praktik terbaik IPv6 untuk memberikan ruang $65.536$ subnet /64.
* Menjamin fleksibilitas pertumbuhan hierarki internal tanpa perlu renumbering di masa depan.
* Penyedia jasa (ISP) tidak terbebani tumpukan tabel routing karena agregasi /48 sangat bersih.

### 9. Rancangan Zona dan Mengapa Subnetting Saja Belum Cukup
* Zona: User (/23), Server (/24), Tamu (/24), IoT (/25), Management (/26).
* Mengapa Belum Cukup: Subnetting hanya membagi batas pengalamatan L3. Tanpa **Access Control List (ACL)**, **Firewall**, atau **VLAN isolation**, router tetap akan meneruskan seluruh trafik antar-subnet secara default.

### 10. Kriteria Audit Rencana Alamat "Layk Produksi"
1. **Bebas Conflict/Overlap:** Terverifikasi 100% bebas dari overlapping IP/subnet.
2. **Hierarki & Agregasi:** Mendukung summarization rute di setiap tingkatan Core/Distribution.
3. **Kepatuhan Standar:** Menggunakan /64 untuk LAN IPv6, /31 atau /127 untuk p2p link.
4. **Kapasitas Cadangan:** Memiliki proyeksi pertumbuhan minimal 3-5 tahun ke depan.
5. **Ketersediaan Dokumentasi:** Terintegrasi penuh dengan sistem IPAM otomatis dan memiliki skema penamaan DNS PTR yang konsisten.

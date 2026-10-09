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

### 1. Perbedaan Jangkauan Kerja Data Link Layer dan Network Layer
* Data Link Layer (L2): Bekerja secara lokal antar-node yang terhubung langsung dalam satu media/subnet fisik yang sama (*hop-to-hop*).
* Network Layer (L3): Bekerja secara lintas jaringan dari perangkat pengirim hingga perangkat penerima akhir di seluruh internet (*end-to-end*).

### 2. Alasan Alamat IP Disebut Logis dan Hierarkis
* Logis: Ditentukan melalui perangkat lunak dan dapat diubah sesuai lokasi jaringan tempat perangkat terhubung (berbeda dengan MAC address yang tertanam permanen di hardware).
* Hierarkis: Memiliki struktur terbagi menjadi Network ID (menunjukkan kelompok/wilayah jaringan) dan Host ID (menunjukkan perangkat spesifik), mirip dengan kode pos dan nomor rumah.

### 3. Lima Fungsi Utama Network Layer
1. Pengalamatan Logis (Logical Addressing - IPv4/IPv6).
2. Pengarahan Rute (Routing).
3. Enkapsulasi dan Dekapsulasi Paket.
4. Pemotongan dan Penyambungan Paket (Fragmentation & Reassembly).
5. Penanganan Error dan Informasi Kontrol (via ICMP).

### 4. Arti Layanan Best Effort
Layanan pengiriman data tanpa jaminan (*unreliable*). Network Layer akan berusaha semaksimal mungkin menyampaikan paket, tetapi tidak menjamin paket pasti sampai, berurutan, atau bebas dari duplikasi. Jaminan keandalan diserahkan ke Transport Layer (TCP).

### 5. Perbedaan Routing dan Forwarding
* Routing: Proses menentukan jalur terbaik dari asal ke tujuan menggunakan algoritma/protokol routing (proses membuat tabel keputusan di *control plane*).
* Forwarding: Proses memindahkan paket dari port masuk ke port keluar pada satu router berdasarkan tabel routing (proses eksekusi di *data plane*).

### 6. Fungsi TTL pada IPv4 dan Hop Limit pada IPv6
Nilai batas yang dikurangi 1 oleh setiap router yang dilewati paket. Fungsinya untuk mencegah paket berputar tiada henti (*routing loop*) di dalam jaringan. Jika nilainya mencapai 0, paket dibuang dan router mengirimkan pesan ICMP Time Exceeded.

### 7. Ukuran Minimum dan Maksimum Header IPv4
* Minimum: 20 byte (tanpa field Options).
* Maksimum: 60 byte (dengan opsi maksimum 40 byte).

### 8. Alasan Checksum Header IPv4 Dihitung Ulang di Setiap Hop
Karena nilai field TTL (Time to Live) selalu berkurang 1 di setiap router yang dilewati. Karena isi header berubah, nilai checksum-nya harus dihitung ulang agar valid di hop berikutnya.

### 9. Analisis Subnet 192.168.10.25/24
* Network ID: `192.168.10.0`
* Broadcast Address: `192.168.10.255`
* Rentang Host Lazim: `192.168.10.1` sampai `192.168.10.254`

### 10. Tiga Blok Alamat Privat RFC 1918
1. Kelas A: `10.0.0.0` sampai `10.255.255.255` (`10.0.0.0/8`)
2. Kelas B: `172.16.0.0` sampai `172.31.255.255` (`172.16.0.0/12`)
3. Kelas C: `192.168.0.0` sampai `192.168.255.255` (`192.168.0.0/16`)

### 11. Perbedaan Alamat Loopback dan Link-Local
* Loopback (`127.0.0.1` / `::1`): Alamat virtual untuk menguji tumpukan protokol TCP/IP internal di dalam perangkat itu sendiri tanpa mengirim data ke media fisik.
* Link-Local (`169.254.0.0/16` / `fe80::/10`): Alamat otomatis yang digunakan untuk komunikasi terbatas antar-perangkat dalam satu domain broadcast/segment fisik yang sama.

### 12. Mengapa IPv6 Tidak Menggunakan Broadcast?
Karena broadcast mengganggu seluruh perangkat di dalam jaringan yang harus memproses paket meski tidak berkepentingan. IPv6 menggantikannya dengan **Multicast** yang lebih efisien dan spesifik, serta **Anycast**.

---

## Level B — Penerapan Konsep

### 1. Pemilihan Rute untuk IP 10.20.8.9
Rute yang dipakai adalah **`10.20.0.0/16`**.
* Alasan: Mengikuti aturan **Longest Prefix Match (LPM)**. Rute `/16` memiliki kecocokan bit paling spesifik (16 bit) dibanding `/8` (8 bit) dan `/0` (default route 0 bit).

### 2. Evaluasi Pernyataan: 172.40.10.5 Adalah Alamat Privat
Pernyataan tersebut **SALAH**. Rentang IPv4 privat Kelas B hanya berkisar dari `172.16.0.0` sampai `172.31.255.255`. Alamat `172.40.10.5` berada di luar rentang privat tersebut, sehingga merupakan **IP Publik**.

### 3. Perubahan Header Saat Melewati Tiga Router
* Header IP: Nilai TTL/Hop Limit berkurang 3 (berkurang 1 di setiap router), Checksum IPv4 dihitung ulang di setiap hop. IP Asal dan IP Tujuan tetap.
* Header Layer 2 (Ethernet): Dilepas dan dibuat ulang secara total di setiap router. Destination MAC dan Source MAC berganti sesuai antarmuka router dan hop berikutnya.

### 4. Diagnosis Host Mendapat IP 169.254.20.8/16 (APIPA)
* Artinya: Host gagal mendapatkan respon dari server DHCP.
* Empat Hipotesis & Bukti:
  1. Server DHCP mati/penuh: Cek ketersediaan IP pool di log server DHCP.
  2. Kabel/Koneksi fisik terputus: Periksa indikator lampu LAN atau status link Wi-Fi.
  3. Masalah VLAN/Trunk: Cek apakah port switch terhubung ke VLAN yang salah tanpa DHCP Relay.
  4. Firewall memblokir port UDP 67/68: Periksa aturan inbound/outbound pada host atau switch.

### 5. Mengapa Fragmentasi Meningkatkan Dampak Kehilangan Paket?
Karena IP tidak memiliki mekanisme pengiriman ulang per fragmen. Jika satu fragmen kecil hilang atau rusak di jalan, seluruh paket IP utuh gagal dirangkai kembali dan terbuang, memaksa Transport Layer (TCP) mengirim ulang seluruh paket besar dari awal.

### 6. Peranan Router pada Fragmentasi IPv4 vs IPv6
* IPv4: Router di tengah jalan berhak memotong (memfragmentasi) paket jika ukuran paket melebihi MTU jalur keluar.
* IPv6: Router **dilarang** memfragmentasi paket. Fragmentasi hanya boleh dilakukan oleh **host pengirim** berbasis mekanisme Path MTU Discovery (PMTUD).

### 7. Cara Kerja Traceroute Menemukan Hop
Traceroute mengirimkan urutan paket (ICMP/UDP) dengan nilai TTL yang dinaikkan secara bertahap mulai dari TTL=1, TTL=2, dst. Setiap router tempat TTL habis (TTL=0) akan membuang paket dan mengirim balik pesan *ICMP Time Exceeded* ke pengirim, sehingga IP router tersebut tercatat.

### 8. Mengapa Kegagalan Ping Belum Tentu Server Mati?
Karena protokol ICMP Echo Request sering kali diblokir oleh firewall server atau router perantara demi alasan keamanan, meskipun layanan utama server (seperti HTTP/HTTPS pada port 80/443) tetap berjalan normal.

### 9. Perbedaan Source NAT, Destination NAT, dan PAT
* Source NAT (SNAT): Mengubah IP asal paket lokal menjadi IP publik (digunakan agar jaringan internal bisa mengakses internet).
* Destination NAT (DNAT): Mengubah IP tujuan paket masuk dari IP publik ke IP server internal (digunakan untuk mempublikasikan server lokal ke internet).
* Port Address Translation (PAT / Masquerade): Mengubah IP asal sekaligus memetakan nomor port TCP/UDP lokal ke satu IP publik secara bersamaan, memungkinkan ribuan perangkat berbagi satu IP publik.

### 10. Alasan NAT Tidak Boleh Disamakan dengan Firewall
NAT diciptakan untuk menghemat stok IP publik dengan menerjemahkan alamat, bukan untuk keamanan. Meski NAT menyembunyikan IP internal secara tidak langsung, NAT tidak memiliki fitur pemeriksaan paket (*stateful inspection*), pencegahan intrusi, atau aturan penyaringan lalu lintas seperti firewall.

### 11. Format Alamat IPv6
* Bentuk Lengkap `2001:db8::5`: `2001:0db8:0000:0000:0000:0000:0000:0005`
* Pemendekan `2001:0db8:0000:0000:0000:00aa:0000:0001`: `2001:db8::aa:0:1`

### 12. Tiga Alamat IPv6 dalam Satu Antarmuka
IPv6 dirancang multi-homed secara alami:
* Link-Local (`fe80::`): Wajib ada untuk komunikasi tetangga lokal (NDP/gateway).
* Global Unicast (GUA): Untuk akses komunikasi utama ke internet publik.
* Temporary Address: Dibuat acak secara berkala khusus untuk trafik keluar demi menjaga privasi pengguna agar perangkat tidak mudah dilacak.

### 13. Alasan DHCPv6 Saja Belum Tentu Menghasilkan Rute Bawaan
Karena protokol DHCPv6 standar tidak membawa informasi Default Gateway. Rute bawaan (default route) pada IPv6 diperoleh secara terpisah melalui pesan **Router Advertisement (RA)** yang dipancarkan oleh router menggunakan protokol NDP.

---

## Level C — Analisis dan Evaluasi

### 1. Analisis Overhead VPN 80 Byte pada MTU 1.500
* Risiko: Paket IP 1.500 byte ditambah overhead 80 byte menjadi 1.580 byte, melebihi MTU Ethernet (1.500 byte). Ini menyebabkan paket terfragmentasi atau dibuang jika bit DF (Don't Fragment) aktif.
* Penanganan: Turunkan nilai **MSS (Maximum Segment Size)** pada TCP menjadi 1.360 byte (`tcp mss-adjust 1360`) atau naikkan MTU jalur fisik jika mendukung Jumbo Frames.

### 2. Diagnosis PMTUD: Buka Halaman Awal Bisa, Unduhan Berhenti
1. Tanda Masalah: Paket kontrol TCP kecil (SYN/ACK) lolos, tetapi paket data besar (payload unduhan) tertahan karena melebihi MTU jalur.
2. Penyebab: Ada router di tengah jalan yang membuang paket besar tetapi pesan *ICMP Type 3 Code 4 (Fragmentation Needed)* diblokir oleh firewall (*Black Hole Router*).
3. Langkah Solusi: Aktifkan fitur *Path MTU Discovery Black Hole Detection* atau terapkan *TCP MSS Clamping* pada router gateway.

### 3. Evaluasi CGNAT (Carrier-Grade NAT)
* Penyedia (ISP): Hemat stok IPv4 publik dan efisiensi biaya operasional.
* Pelanggan: Merugi karena tidak bisa melakukan port forwarding (sulit untuk hosting server, CCTV, online gaming/P2P, atau akses remote).
* Penyelidik Insiden: Sangat menyulitkan pelacakan kejahatan siber karena satu IP publik digunakan bersama oleh ribuan pelanggan pada waktu yang sama.

### 4. Dual-Stack vs IPv6-only + NAT64 untuk Kampus Baru
* Dual-Stack: Lebih kompatibel dengan sistem lama, namun boros alokasi IPv4 dan menambah beban kerja router (*dual routing table*).
* IPv6-only + NAT64 (Rekomendasi): **Sangat Baik untuk Jaringan Baru.** Menghemat IPv4 total, menyederhanakan manajemen jaringan internal, dan menggunakan NAT64/DNS64 hanya saat pengguna mengakses konten IPv4 lama.

### 5. Cara Kerja 464XLAT untuk Aplikasi Literal IPv4
1. **CLAT (Customer-side Translator):** Menerjemahkan paket IPv4 dari aplikasi lokal menjadi paket IPv6 di perangkat pengguna.
2. **Jaringan IPv6:** Paket dikirimkan melintasi jaringan kampus yang murni berbasis IPv6.
3. **PLAT (Provider-side Translator / NAT64):** Menerjemahkan kembali paket IPv6 tersebut menjadi IPv4 publik saat keluar ke internet.

### 6. Kebijakan ICMPv6 Aman untuk NDP dan PMTUD
Izinkan jenis pesan ICMPv6 berikut pada firewall:
* Router Solicitation (Type 133) & Router Advertisement (Type 134) - *Link-Local only*.
* Neighbor Solicitation (Type 135) & Neighbor Advertisement (Type 136) - *Link-Local only*.
* Packet Too Big (Type 2) - *Sangat penting untuk PMTUD*.
* Destination Unreachable (Type 1) & Time Exceeded (Type 3).
* Blokir jenis pesan ICMPv6 lain yang tidak dibutuhkan dari luar jaringan.

### 7. Risiko Firewall Ketat IPv4 tetapi Membiarkan IPv6
* Risiko: Terjadi kebocoran keamanan (*IPv6 Bypass/Bypass Attack*). Penyerang dapat mengidentifikasi dan meretas server internal melewati jalur IPv6 yang tidak diawasi.
* Rencana Remediation: Samakan aturan (parity policy) firewall IPv6 dengan IPv4, aktifkan *RA Guard* pada switch, serta jalankan pemindaian kerentanan pada tumpukan IPv6.

### 8. Memeriksa Laporan Adopsi IPv6 (47% vs 55%)
Informasi metodologis yang wajib diperiksa:
1. Sumber Sampel Data: Apakah diukur dari lalu lintas pengguna (seperti statistik Google/Akamai) atau dari alokasi sertifikat IP/ISP (seperti APNIC).
2. Cakupan Geografis: Apakah sampel mencakup seluruh penyedia ISP atau hanya operator seluler utama.
3. Jenis Perangkat: Apakah mengukur lalu lintas seluler (*mobile*) saja atau memasukkan jaringan kabel (*fixed broadband*).

### 9. Analisis Rute /24 Memilih Mengalahkan Agregat /16
Dalam prinsip dasar IP Routing, aturan **Longest Prefix Match (LPM)** selalu diutamakan. Rute spesifik `/24` memiliki panjang prefix kecocokan bit yang lebih panjang dibanding rute agregat `/16`, sehingga seluruh lalu lintas data akan diarahkan ke jalur `/24` meskipun rute tersebut salah atau palsu (*Route Hijacking*).

### 10. Bukti Minimum Diagnosis "Situs Tidak Dapat Dibuka"
1. Uji DNS: Jalankan `nslookup nama-situs.com`. Jika IP tidak muncul = **Gangguan DNS**.
2. Uji Routing: Jalankan `traceroute IP-situs`. Jika terhenti sebelum gateway = **Gangguan Routing**.
3. Uji Firewall: Jalankan `nc -zv IP-situs 443` atau `Test-NetConnection`. Jika Ping sukses tapi Port 443 Timeout = **Gangguan Firewall**.
4. Uji Aplikasi: Jalankan `curl -I https://nama-situs.com`. Jika terkoneksi tapi mengembalikan error HTTP 500/503 = **Gangguan Aplikasi Server**.

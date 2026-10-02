# Denial-Of-Service-App-Layer-Evasion-Detection-Dan-Mitigasi-Waf
> Evaluasi dilakukan untuk mengetahui kemampuan sistem dalam mempertahankan ketersediaan layanan, mendeteksi pola trafik berbahaya, dan memblokir permintaan yang menyerupai trafik normal tetapi memiliki karakteristik serangan.
<br></br>
# Ringkasan
- [Tools](#tools)
- [Tujuan Evaluasi](tujuan-evaluasi)
- [Hasil Temuan](#hasil-temuan)
- [Rekomendasi Mitigasi](rekomendasi-mitigasi)
<br></br>
## Tools
- [Wget](https://www.kali.org/tools/wget/)
- [Web lab]()
- [Cyberchef](https://toolbox.itsec.tamu.edu/) 
<br></br>
## Tujuan Evaluasi
1.	Mengidentifikasi dampak serangan Denial of Service pada lapisan aplikasi terhadap ketersediaan dan performa layanan.
2.	Mengukur kemampuan aplikasi dan infrastruktur dalam mempertahankan layanan ketika menerima peningkatan trafik atau permintaan HTTP yang tidak wajar.
3.	Menganalisis efektivitas mekanisme deteksi terhadap pola serangan yang disamarkan menyerupai trafik pengguna normal.
4.	Menguji kemampuan WAF dalam mendeteksi, mencatat, membatasi, dan memblokir permintaan berbahaya.
5.	Menilai perubahan kondisi sistem sebelum dan sesudah penerapan mitigasi berdasarkan indikator seperti waktu respons aplikasi, penggunaan CPU dan memori, jumlah request per detik, tingkat keberhasilan atau kegagalan request, jumlah request yang diblokir, detection rate, false positive rate; dan ketersediaan layanan.
6.	Menemukan kelemahan pada konfigurasi WAF, application server, endpoint, atau mekanisme monitoring yang dapat dimanfaatkan oleh penyerang.
7.	Menyusun rekomendasi tindakan mitigasi untuk memperkuat pertahanan terhadap App-Layer DoS dan teknik penghindaran deteksi.
8.	Memastikan seluruh pengujian dilakukan secara terkendali, terdokumentasi, dan hanya pada sistem yang telah memperoleh izin pengujian.
<br></br>
## Hasil Temuan
Dari bukti log yang diunduh dari web lab, diperoleh 2000 baris log. Terdapat beberapa IP yang dominan mengirim request. Juga diperoleh informasi bahwa penyerang mengubah-ubah User-Agent pada setiap request (User-Agent Spoofing). Lonjakan trafik dari IP penyerang tersebut dapat diklasifikasikan sebagai Application-Layer DoS (HTTP Flood). Hal ini dikarenakan lonjakan trafik dari IP penyerang dapat membebani web server yang menyebabkan terkurasnya sumber daya seperti RAM dan CPU. Lonjakan trafik ini bukan lonjakan pengguna sah (Flash Crowd / Legitimate Traffic Burst) karena lonjakan pengguna sah (Flash Crowd / Legitimate Traffic Burst) terjadi jika dalam log terdapat request dari berbagai alamat IP dan user agent yang berbeda. Bukan satu alamat IP dengan user-agent yang beragam.

Pada bukti log serangan terdapat metrik Latency : <i>(ms)</i> yang menunjukkan berapa lama server membutuhkan waktu untuk menyelesaikan request. Dari log, terdapat serangan yang menargetkan endpoint spesifik seperti ekspor data, pencarian, atau login. Latency cenderung meningkat karena proses request yang memicu berkurangnya resource CPU/RAM server.

Pada endpoint ekspor data, misalnya, proses ini melibatkan pengambilan ribuan hingga jutaan data dari database, kemudian melakukan filtering hingga sorting untuk membentuk baik file PDF maupun Excel yang kemudian disimpan dalam RAM lalu dikirimkan melalui koneksi HTTP. Akibatnya, sumber daya berupa CPU/RAM yang dimiliki web server menjadi terbebani, terlebih lagi jika menjalankan lebih dari satu request atau beberapa request secara bersamaan. Demikian juga pada endpoint pencarian dan login. Pada endpoint pencarian, database dipaksa untuk membaca banyak data melalui request. Hal ini dapat membebani resource server. 

Sedangkan pada endpoint login, request login berulang dapat menyebabkan database kehabisan koneksi. Hal ini dikarenakan proses login menjalankan beberapa proses sekaligus. Seperti mencari akun di database, memverifikasi password, membuat session_token, memeriksa status akun, menggunakan Multi-Factor Authentication (MFA), hingga pencatatan aktivitas ke log. Jika dibandingkan dengan serangan Volumetric DDoS (Network Layer) biasa, serangan Volumetric DDoS (Network Layer) biasa lebih berfokus pada menyerang lalu lintas jaringan, yang menyebabkan bandwidth exhaustion atau terjadinya trafik pada lalu lintas jaringan, tetapi tidak berpotensi untuk membuat web server berhenti beroperasi. 

Pada log yang sama, terdapat anomali pada endpoint /search di mana Intrusion Detection System (IDS) perusahaan gagal memblokir serangan injeksi (SQLi/LFI). Ditemukan baris log yang mengandung kueri parameter dengan karakter % (persen) yang panjang dan berulang. Dari baris log tersebut dilakukan decoding menggunakan CyberChef, dan setelah berhasil didekode sepenuhnya, diperoleh payload teks terang (plaintext) yang meliputi <i>1=1, ' OR, script, ../, UNION SELECT, passwd, <script>, etc/passwd.</i>

Teknik encoding seperti URL/Hex Encoding dan fragmentation sering kali berhasil mengecoh mesin IDS/IPS generasi lama yang hanya menggunakan pencocokan pola (String/Signature Matching). Hal ini disebabkan oleh mesin IDS generasi lama yang hanya sebatas mencocokkan teks polos atau string. Sedangkan teknik encoding seperti  URL/Hex Encoding dapat mengubah bentuk payload dan fragmentasi dapat memecah payload menjadi beberapa paket sehingga pola serangan tidak dapat terbaca secara utuh hanya dalam satu kali pemeriksaan.
<br></br>
## Rekomendasi Mitigasi
- Implementasikan Rate Limiting
- Implementasi Normalisasi & Pemblokiran Evasion

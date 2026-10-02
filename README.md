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
5.	Menilai perubahan kondisi sistem sebelum dan sesudah penerapan mitigasi berdasarkan indikator seperti, waktu respons aplikasi, penggunaan CPU dan memori, jumlah request per detik, tingkat keberhasilan atau kegagalan request, jumlah request yang diblokir, detection rate, false positive rate; dan ketersediaan layanan.
6.	Menemukan kelemahan pada konfigurasi WAF, application server, endpoint, atau mekanisme monitoring yang dapat dimanfaatkan oleh penyerang.
7.	Menyusun rekomendasi tindakan mitigasi untuk memperkuat pertahanan terhadap App-Layer DoS dan teknik penghindaran deteksi.
8.	Memastikan seluruh pengujian dilakukan secara terkendali, terdokumentasi, dan hanya pada sistem yang telah memperoleh izin pengujian.
<br></br>
## Hasil Temuan
Dari bukti log yang diunduh dari web lab, diperoleh 2000 baris log. Terdapat beberapa IP yang dominan mengirim request.
<br></br>
## Rekomendasi Mitigasi
<br></br>

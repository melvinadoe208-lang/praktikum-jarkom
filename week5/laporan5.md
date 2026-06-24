# LAPORAN PRAKTIKUM JARKOM
## MODUL 5 UDP
## Tujuan Praktikum
Mahasiswa dapat menginvestigasi cara kerja protokol UDP menggunakan Wireshark 

## Langkah Kerja
1. Persiapan: Membuka aplikasi Wireshark dan memilih interface jaringan yang aktif (Wi-Fi atau Ethernet).
2. Pembersihan Cache: Menjalankan perintah ipconfig /flushdns pada Command Prompt untuk memastikan resolusi nama dilakukan melalui jaringan, bukan memori lokal.
3. Trigger Trafik: Menjalankan perintah nslookup google.com untuk memicu pengiriman paket UDP.
4. Sniffing & Filtering: Menghentikan capture Wireshark dan menerapkan filter dns untuk mengisolasi paket UDP yang relevan.
5. Analisis: Membedah bagian User Datagram Protocol pada panel detail paket untuk mengamati isi header.

## Hasil dan Analisis Praktikum
## Capture Trafik UDP
![Gambar 1](../assets/image/gambar%201%20week%205.png)
![Gambar 2](../assets/image/gambar%202%20week%205.png)

Analisis Teknis: Berdasarkan capture, teridentifikasi penggunaan Protocol Number 17 (0x11) pada IP Header yang mengarahkan data ke protokol UDP. Transaksi dimulai dengan Standard Query klien ke server DNS. Penggunaan UDP terbukti sangat efektif karena resolusi nama dilakukan secara instan tanpa perlu sinkronisasi koneksi awal, sehingga menghemat sumber daya jaringan.

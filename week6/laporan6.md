# LAPORAN PRAKTIKUM JARKOM
## MODUL 6 TCP
## Tujuan Praktikum
Mahasiswa dapat menginvestigasi cara kerja protokol TCP menggunakan Wireshark 

## Langkah Kerja
1. Download file alice.txt dari server laboratorium.
2. Buka halaman unggah file di gaia.cs.umass.edu.
3. Jalankan Wireshark capture dan upload file alice.txt.
4. Stop capture dan gunakan filter: tcp && ip.addr == 128.119.245.12.
5. Analisis handshake, segmen data, dan grafik time-sequence.

## Hasil dan Analisis Praktikum
Identitas Koneksi TCP
![Gambar 1](../assets/image/gambar%201%20week%206.png)
Berdasarkan capture, parameter identitas koneksi adalah:
- Client IP: 192.168.0.170 (Port: 59805)
- Server IP: 128.119.245.12 (Port: 443 / HTTPS)

Paket SYN ( Client -> Server )
![Gambar 2](../assets/image/gambar%202%20week%206.png)
- Sequence Number: 0 (relative)
- Flags: SYN
- MSS: 1460 bytes   
- Window Scale: x256

![Gambar 3](../assets/image/gambar%203%20week%206.png)
- Sequence Number: 0
- Acknowledgment: 1
- Flags: SYN, ACK

HTTP POST Segment ( Data Transfer )
![Gambar 4](../assets/image/gambar%204%20week%206.png)
Detail segmen pengiriman daya pada Frame 163:
- Frame Number: 163
- Source / Destination: 192.168.0.170:53639 -> 128.119.245.12:80
- Sequence Number: 1
- Payload Size: 728 bytes
- Flags: PSH, ACK
- Window Size: 255 bytes

Flow Control & Window Size Analysis
![Gambar 5](../assets/image/gambar%205%20week%206.png)
- Perhitungan Window Size: 65535 x 256 = 16.776.960 bytes
- Analisis: Tidak ditemukan Zero-window condition, buffer server selalu tersedia untuk menerima data tanpa hambatan.

Retransmisi & Pola Acknowledgment
![Gambar 6](../assets/image/gambar%206%20week%206.png)
- Retransmisi: Ditemukan paket pada No. 145, 297, 298, 424, 446.
- ACK Type: Cumulative ACK
- Frequency: Delayed ACK (~ 1 ACK per 2 segmen)
- SACK: Enabled

Analisis Congestion Control ( Stevens Graph )
![Gambar 7](../assets/image/gambar%207%20week%206.png)






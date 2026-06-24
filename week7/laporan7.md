# LAPORAN PRAKTIKUM JARKOM
## MODUL 7 TCP
## Tujuan Praktikum
1. Mahasiswa bisa membuat program berbasis socket UDP
2. Mahasiswa bisa membuat program berbasis socket TCP

## Langkah Kerja
1. Mempersiapkan lingkungan Python pada VS Code.
2. Membuat file UDPServer.py dan UDPClient.py. Jalankan server terlebih dahulu, lalu kirim pesan teks dari client.
3. Membuat file TCPServer.py dan TCPClient.py. Amati proses inisiasi koneksi sebelum data ditransfer.
4. Melakukan pengujian Multiple Clients pada kedua protokol untuk melihat perbedaan penanganan antrian.
5. Mendokumentasikan hasil eksekusi terminal dan menganalisis perbedaan teknisnya.

## Hasil dan Analisis Praktikum
**Kode Program UDP Server**
```python
from socket import *

serverPort = 12000
serverSocket = socket(AF_INET, SOCK_DGRAM)
serverSocket.bind(('', serverPort))

print("The server is ready to receive")

while True:
message, clientAddress = serverSocket.recvfrom(2048)
modifiedMessage = message.decode().upper()
serverSocket.sendto(modifiedMessage.encode(), clientAddress)
```

**Penjelasan Alur Program**
- Inisialisasi: Server membuat socket dengan tipe SOCK_DGRAM (UDP) dan melakukan bind ke port 12000.
- Proses: Server masuk ke dalam infinite loop untuk menunggu paket. Karena UDP connectionless, server tidak perlu menerima koneksi, melainkan langsung membaca data dan alamat pengirim menggunakan recvfrom().
- Output: Data diubah menjadi huruf kapital (uppercase) lalu dikirim balik ke alamat client yang spesifik menggunakan sendto().

**Kode Program UDP Client**

```python
from socket import *

serverName = 'localhost'
serverPort = 12000

clientSocket = socket(AF_INET, SOCK_DGRAM)
message = input('Input lowercase sentence: ')
clientSocket.sendto(message.encode(), (serverName, serverPort))

modifiedMessage, serverAddress = clientSocket.recvfrom(2048)
print(modifiedMessage.decode())

clientSocket.close()
```

**Penjelasan Alur Program**
- Transmisi: Client langsung mengirimkan data ke IP server dan port 12000 tanpa ada proses jabat tangan (handshake) terlebih dahulu.
- Respon: Setelah mengirim, client menunggu balasan paket dari server di port yang sama.
- Penutup: Setelah data diterima dan ditampilkan, socket langsung ditutup dengan close().

**Hasil Eksekusi UDP**
![Gambar 1](../assets/image/gambar%201%20week%207.png)

**Analisis Hasil Eksekusi**
- Server Ready: Terminal server menampilkan pesan "The server is ready to receive", menunjukkan socket telah berhasil di-bind ke port 12000.
- Input Client: Saat client menginput teks lowercase (contoh: hello world), data langsung dikirim tanpa ada fase koneksi.
- Output: Server menerima datagram, memprosesnya menjadi uppercase, dan client langsung menampilkan hasilnya. Hal ini menunjukkan komunikasi one-way yang sangat cepat karena tidak ada manajemen sesi.

** Kode Program TCP Server
```python
from socket import *

serverPort = 12000
serverSocket = socket(AF_INET, SOCK_STREAM)
serverSocket.bind(('', serverPort))
serverSocket.listen(1)

print('The server is ready to receive')

while True:
connectionSocket, addr = serverSocket.accept()
sentence = connectionSocket.recv(1024).decode()
capitalizedSentence = sentence.upper()
connectionSocket.send(capitalizedSentence.encode())
connectionSocket.close()
```

**Penjelasan Alur Program**
- Setup: Server menggunakan tipe SOCK_STREAM (TCP) dan memanggil listen(1) untuk mulai mendengarkan permintaan koneksi masuk.
- Handshake: Saat client mencoba terhubung, server menjalankan accept() yang secara otomatis melakukan three-way handshake.
- Sesi: Fungsi accept() menghasilkan socket baru (connectionSocket) yang khusus digunakan hanya untuk satu client tersebut, sehingga jalur komunikasi lebih terjamin (reliable).

**Kode Program TCP Client**
```python
from socket import *

serverName = 'localhost'
serverPort = 12000

clientSocket = socket(AF_INET, SOCK_STREAM)
clientSocket.connect((serverName, serverPort))

sentence = input('Input lowercase sentence: ')
clientSocket.send(sentence.encode())

modifiedSentence = clientSocket.recv(1024)
print('From Server:', modifiedSentence.decode())

clientSocket.close()
```

**Penjelasan Alur Program:**
- Koneksi: Client harus memanggil connect() terlebih dahulu. Jika server tidak aktif atau port salah, maka akan terjadi error karena koneksi tidak terbentuk.
- Transfer: Setelah status menjadi Established, client mengirim data menggunakan send() (tanpa perlu menyebutkan alamat lagi karena jalur sudah terbentuk).
- Respon: Client menerima balasan dari server melalui fungsi recv().

**Hasil Eksekusi TCP**
![Gambar 2](../assets/image/gambar%202%20week%207.png)

**Analisis Hasil Eksekusi**
- Connection Established: Berbeda dengan UDP, saat client dijalankan, terjadi proses handshake terlebih dahulu. Jika server belum jalan, client akan langsung error/refused.
- Reliable Data: Setelah koneksi stabil, data dikirim melalui jalur khusus (connectionSocket). Output yang diterima client (contoh: NETWORKING LAB) dipastikan sampai secara utuh karena adanya mekanisme kontrol pengiriman pada protokol TCP.
- Closing Connection: Setelah pesan diterima, koneksi antara client dan server tersebut diputus secara formal menggunakan close(), sementara server utama tetap listening untuk client berikutnya.

## Perbandingan UDP vs TCP
**Perbedaan Implementasi**
| Aspek | UDP (User Datagram) | TCP (Transmission Control) |
| :--- | :--- | :--- |
| **Socket Type** | `SOCK_DGRAM` | `SOCK_STREAM` |
| **Koneksi** | Tidak perlu `connect()` | Perlu `connect()` |
| **Server Socket** | 1 socket untuk semua client | 2 socket (`server` & `connection`) |
| **Metode Kirim** | `sendto()` (Perlu info alamat) | `send()` (Langsung ke stream) |

**Perbedaan Hasil Eksekusi**
| Karakteristik | UDP | TCP |
| :--- | :--- | :--- |
| **Kecepatan** | Sangat tinggi (Low Latency) | Menengah (Handshake Delay) |
| **Reliability** | Rendah (Bisa paket hilang) | Tinggi (Jaminan pengiriman) |
| **Order** | Tidak menjamin urutan | Data sampai secara berurutan |
---
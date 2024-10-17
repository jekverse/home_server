# Web Server Architecture Overview

Ini adalah panduan singkat untuk memahami arsitektur dasar dari sebuah **web server** yang bertugas untuk menjalankan website dan melayani permintaan klien melalui protokol HTTP/HTTPS.

## Diagram Arsitektur Server Web

Berikut adalah diagram sederhana untuk menggambarkan alur arsitektur server web:

```text
[ Client (Browser) ]
        |
        v
   [ DNS Server ] ---------------------
        |                              |
        v                              v
  [ Firewall ]                   [ Web Server ]
        |                              |
        v                              v
   [ Load Balancer ]               [ Application Server (Optional) ]
        |                              |
        v                              v
   [ Caching Layer ]               [ Database Server ]
        |                              |
        v                              v
   [ Web Content ]                  [ Data Response ]
```

## Komponen Utama dalam Arsitektur Web Server

1. **Client (Browser)**:
   - Pengguna membuka browser dan mengakses website melalui URL (contoh: `http://example.com`).
   - Browser mengirimkan **request HTTP** ke server, meminta file atau halaman tertentu.

2. **DNS (Domain Name System)**:
   - Browser pertama-tama perlu menerjemahkan nama domain (seperti `example.com`) menjadi alamat IP server menggunakan DNS.
   - DNS akan mengembalikan alamat IP yang sesuai dengan domain yang diminta.

3. **Firewall**:
   - Sebelum permintaan mencapai server, biasanya melewati **firewall**. 
   - Firewall mengontrol lalu lintas jaringan dan hanya mengizinkan permintaan yang sah (contohnya, dari port 80 untuk HTTP atau port 443 untuk HTTPS).

4. **Web Server Software**:
   - Ini adalah komponen inti yang bertanggung jawab untuk menangani semua permintaan HTTP/HTTPS dari klien.
   - **Pilihan Web Server** yang paling umum digunakan:
     - **Nginx**: Web server modern yang cepat dan efisien, juga digunakan sebagai **reverse proxy**.
     - **Apache**: Web server populer dengan arsitektur modular, mendukung berbagai bahasa dan fleksibel.
   - **Tugas Web Server**:
     - Melayani **file statis** seperti HTML, CSS, gambar, dan JavaScript.
     - Menghubungkan ke **server aplikasi** untuk konten dinamis (misalnya Python, PHP, atau Node.js).
     - Mengelola **SSL/TLS** untuk mengamankan komunikasi (HTTPS).

5. **Server Aplikasi (Opsional)**:
   - Jika website membutuhkan **konten dinamis**, web server akan meneruskan permintaan ke **server aplikasi** seperti:
     - **PHP-FPM**: Untuk aplikasi berbasis PHP.
     - **Gunicorn + Flask/Django**: Untuk aplikasi berbasis Python.
     - **Node.js**: Untuk aplikasi berbasis JavaScript.
   - Server aplikasi menjalankan logika bisnis dan menghasilkan respons dinamis (misalnya, halaman yang dihasilkan dari database).

6. **Database**:
   - **Database** menyimpan data yang diperlukan oleh aplikasi web.
   - Umumnya digunakan **MySQL**, **PostgreSQL**, atau **SQLite**.
   - Server aplikasi terhubung ke database untuk mengambil atau menyimpan data (contoh: informasi pengguna, postingan blog, dll.).

7. **Reverse Proxy (Opsional)**:
   - Web server seperti **Nginx** dapat juga berfungsi sebagai **reverse proxy**, meneruskan permintaan ke beberapa **server backend** dan mendistribusikan beban (load balancing).

8. **Load Balancer (Opsional)**:
   - Jika website memiliki lalu lintas tinggi, **load balancer** dapat digunakan untuk mendistribusikan beban kerja ke beberapa server web atau aplikasi.
   - Ini membantu dalam **skalabilitas** dan **redundansi** (failover).

9. **Caching Layer (Opsional)**:
   - Untuk meningkatkan performa, banyak arsitektur server web menggunakan **caching** (misalnya, **Redis** atau **Varnish**).
   - Ini menyimpan versi sementara dari halaman web sehingga permintaan berikutnya dapat dilayani lebih cepat tanpa harus memproses ulang dari awal.

10. **Firewall Eksternal & Sertifikat SSL/TLS**:
    - Setelah server web memproses permintaan, jika website menggunakan **HTTPS**, data akan dienkripsi menggunakan sertifikat **SSL/TLS** sebelum dikirim kembali ke browser.
    - **Firewall eksternal** (jika ada) memeriksa dan menyaring permintaan masuk dan keluar, memberikan lapisan perlindungan tambahan.

## Alur Kerja (Request Lifecycle)

1. **Browser Request**: 
   - Pengguna mengetik URL di browser (misalnya `https://example.com`) dan browser mengirimkan **request HTTP/HTTPS** ke server web.
   
2. **DNS Resolution**:
   - Nama domain (`example.com`) diterjemahkan menjadi alamat IP oleh server DNS.
   
3. **Firewall**: 
   - Permintaan melewati **firewall**, yang mengizinkan atau memblokir berdasarkan aturan keamanan.

4. **Web Server**:
   - Web server menerima permintaan dan:
     - Jika permintaan untuk **file statis**, web server langsung mengirimkan file tersebut ke browser.
     - Jika permintaan untuk **konten dinamis**, web server meneruskan ke **server aplikasi** (misalnya aplikasi Python atau PHP).
   
5. **Server Aplikasi** (Jika Konten Dinamis):
   - **Server aplikasi** (misalnya Flask atau Django) memproses permintaan, mungkin mengambil data dari **database**, lalu mengembalikan **response** ke web server.

6. **Database Query** (Jika Diperlukan):
   - Server aplikasi menghubungi **database** untuk mengambil atau menyimpan data.
   
7. **Response**:
   - Web server mengirimkan **response HTTP/HTTPS** ke browser.
   - Jika website menggunakan **HTTPS**, data dienkripsi sebelum dikirim ke browser.

8. **Browser Display**:
   - Browser menampilkan halaman web yang diminta ke pengguna.

## Teknologi yang Digunakan

- **Nginx** atau **Apache** sebagai web server.
- **Flask**, **Django** (untuk Python), atau **PHP** untuk server aplikasi.
- **MySQL**, **PostgreSQL**, atau **SQLite** untuk database.
- **Redis**, **Varnish** sebagai caching layer untuk meningkatkan performa.
- **SSL/TLS** untuk keamanan komunikasi melalui HTTPS.
- **Docker** (opsional) untuk mengisolasi aplikasi dalam container.

## Penutup

Dengan arsitektur ini, Anda dapat membangun web server yang melayani konten statis dan dinamis, mengelola permintaan secara efisien, dan menjaga keamanan melalui sertifikat SSL/TLS. Penggunaan reverse proxy dan caching layer juga membantu meningkatkan performa dan skalabilitas aplikasi web Anda.

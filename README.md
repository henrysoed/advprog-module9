## Reflection Module 9

### 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

+ Unary RPC: Melibatkan pengiriman satu permintaan dan menerima satu respons. Cocok untuk operasi sederhana yang tidak memerlukan pertukaran data kontinu, seperti pengambilan item dari database atau otentikasi pengguna, di mana klien hanya perlu hasil setelah operasi selesai. Contoh dalam tutorial ini adalah Layanan Pembayaran (Payment Service).
+ Server Streaming RPC: Klien mengirim satu permintaan, dan server mengirim kembali aliran respons. Ideal untuk skenario di mana server perlu terus-menerus mengirim data atau jumlah data yang besar ke klien, seperti pembaruan real-time atau pengiriman data besar dan berkelanjutan. Contoh dalam tutorial ini adalah Layanan Transaksi (Transaction Service).
+ Bi-directional Streaming RPC: Memungkinkan klien dan server untuk saling mengirim aliran data dalam sesi yang sama. Cocok untuk komunikasi interaktif real-time yang membutuhkan komunikasi dua arah secara kontinu. Contoh dalam tutorial ini adalah Layanan Chat (Chat Service).

### 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

Dalam mengimplementasikan layanan gRPC di Rust, perhatian keamanan dari autentikasi hingga enkripsi data sangatlah penting. Autentikasi merupakan langkah pertama untuk mengidentifikasi entitas yang berhak mengakses sistem. Penggunaan mekanisme seperti token JWT atau autentikasi dua arah dengan TLS diperlukan untuk memverifikasi identitas sebelum memberikan akses ke layanan. Selanjutnya, otorisasi harus diterapkan untuk memastikan bahwa pengguna memiliki izin yang tepat untuk melakukan tindakan tertentu, seperti mengakses atau memodifikasi sumber daya. Untuk mengimplementasikan otorisasi, penggunaan middleware dapat menjadi solusi yang efektif. Selain itu, enkripsi data melalui TLS juga krusial untuk menjaga keamanan proses komunikasi antara klien dan server.

### 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

+ Manajemen Memori dan Asinkronitas: Manajemen kepemilikan dan siklus hidup data Rust perlu dikelola dengan baik untuk menghindari kebocoran memori atau kondisi kompetisi (race conditions).
+ Keamanan (Enkripsi dan Otentikasi): Memastikan komunikasi yang aman terus-menerus, terutama untuk data sensitif seperti pesan pribadi dalam aplikasi obrolan (chat applications).
+ Concurrency: Menangani beberapa aliran pesan secara bersamaan untuk memastikan komunikasi yang efisien.

### 4. What are the advantages and disadvantages of using the tokio_stream::wrappers::ReceiverStream for streaming responses in Rust gRPC services?

Kelebihan:
+ Integrasi yang mulus dengan Tokio untuk manajemen aliran asinkron.
+ Cocok dengan ekosistem asinkron Rust.

Kekurangan:
+ Potensi overhead karena kompleksitas tambahan.
+ Bergantung pada runtime Tokio, membatasi portabilitas aplikasi.

### 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

+ Pemisahan Modul: Membagi fungsi layanan gRPC ke dalam modul atau crate terpisah untuk pengelolaan dan penggunaan kembali yang lebih mudah.
+ Definisi Trait: Menggunakan trait untuk mendefinisikan fungsionalitas bersama yang dapat diimplementasikan di berbagai komponen.
+ Penggunaan Pustaka Eksternal: Memanfaatkan pustaka eksternal untuk tugas-tugas umum guna mengurangi duplikasi kode dan meningkatkan modularitas.

### 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

+ Validasi data masukan untuk memastikan keabsahan informasi pembayaran.
+ Integrasi yang aman dengan gateway pembayaran untuk pemrosesan transaksi.
+ Pertimbangkan penggunaan server streaming untuk menangani data transaksi dalam jumlah besar dan berkelanjutan secara efisien.

### 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

Penggunaan gRPC sebagai protokol komunikasi berdampak besar pada arsitektur dan desain sistem terdistribusi. Dengan menggunakan definisi yang jelas dalam file proto, gRPC memudahkan interaksi antarmodul tanpa perlu konfigurasi manual metode HTTP seperti yang biasanya dilakukan dalam pendekatan REST. Dengan berjalan di atas HTTP/2, gRPC memungkinkan komunikasi yang lebih efisien dan cepat antara layanan-layanan dengan overhead yang rendah dan kemampuan streaming yang unggul. Fitur-fitur ini membantu sistem yang mengadopsi gRPC berkomunikasi secara lebih efektif dan efisien, meningkatkan kinerja dan memudahkan integrasi antar berbagai komponen sistem, sehingga memperkuat interoperabilitas dengan teknologi dan platform lainnya.

### 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

Kelebihan HTTP/2:
+ Multiplexing yang efisien mengurangi latency dan meningkatkan performa.
+ Dukungan superior untuk streaming, ideal untuk komunikasi bi-directional dalam gRPC.

Kekurangan HTTP/2:
+ Kompleksitas implementasi dan persyaratan enkripsi SSL/TLS, yang dapat menambah biaya dan administrasi.
+ Bergantung pada pustaka seperti Tokio untuk dukungan HTTP/2 penuh.

### 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

Model REST API menggunakan pendekatan request-response yang tidak mendukung komunikasi real-time secara native, beroperasi secara stateless dimana setiap request harus lengkap dan mandiri. Sebaliknya, gRPC mendukung streaming dua arah yang memungkinkan komunikasi real-time yang lebih responsif. Dengan memanfaatkan HTTP/2, gRPC memungkinkan transmisi data yang efisien dan efektif melalui streaming yang bisa berlangsung dua arah antara client dan server, sangat cocok untuk aplikasi yang memerlukan pertukaran data secara langsung dan terus-menerus, seperti dalam aplikasi chat atau pembaruan status real-time.

### 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Pendekatan berbasis skema dari gRPC, yang menggunakan Protocol Buffers, menawarkan keunggulan dalam efisiensi pengiriman data karena formatnya yang ringkas dan terstruktur, yang memungkinkan validasi otomatis dan serialisasi data yang cepat dan tipe data yang ketat. Berbeda dengan REST API yang menggunakan JSON, yang lebih fleksibel dan tidak bergantung pada skema yang ketat, memudahkan pengembang untuk mengirimkan berbagai jenis data tanpa perlu definisi skema terlebih dahulu. Namun, fleksibilitas ini bisa meningkatkan risiko kesalahan pada data dan kurangnya ketepatan dalam validasi data, serta mungkin lebih lambat dalam pemrosesan karena JSON menggunakan format teks yang lebih besar dan membutuhkan parsing yang lebih kompleks.
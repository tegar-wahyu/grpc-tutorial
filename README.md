# Module 9 - High Level Networking
## Reflection
Tegar Wahyu Khisbulloh (2206082032) - Pemrograman Lanjut A
### 1.  What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
> -   **Unary RPC** adalah model request-response sederhana, di mana client mengirimkan satu request dan menerima satu response dari server. Unary RPC cocok untuk operasi sederhana yang tidak memerlukan streaming data.
> -   **Server Streaming RPC**: Client mengirimkan satu request, dan server merespons berupa stream of messages. Berguna untuk skenario di mana server perlu mengirimkan sejumlah besar data atau terus memperbarui client dengan informasi baru, seperti umpan real-time data feeds atau log streaming.
> -   **Bidirectional Streaming RPC**: Baik client maupun server dapat mengirimkan aliran pesan satu sama lain, memungkinkan komunikasi real-time. Berguna untuk skenario seperti aplikasi chat, transfer data interaktif, atau operasi yang berjalan lama di mana kedua pihak perlu saling bertukar data secara berkelanjutan.

### 2.  What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
> -   **Autentikasi**: Implementasi mekanisme autentikasi yang tepat, seperti JWT (JSON Web Token) atau protokol autentikasi standar industri lainnya, untuk memverifikasi identitas client.
> -   **Otorisasi**: Implementasi Role-based access control (RBAC) atau mekanisme otorisasi lainnya untuk memastikan bahwa client yang diautentikasi hanya dapat melakukan tindakan yang diizinkan.
> -   **Enkripsi Data**: Menggunakan Transport Layer Security (TLS) atau protokol enkripsi lainnya untuk mengamankan saluran komunikasi antara client dan server, melindungi data sensitif dari kemungkinan diubah.
> -   **Validasi Input**: Validasi dan sanitasi semua input pengguna untuk mencegah serangan injeksi, seperti SQL injection.

### 3.  What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
> Tantangan yang muncul dengan penggunaan bidirectional streaming di Rust gRPC mencakup pemeliharaan state yang memerlukan penjagaan urutan pesan. Menangani pemutusan dan penyambungan kembali koneksi dengan baik sambil mempertahankan state yang benar dan memastikan pengiriman pesan menjadi kompleks. 
>  Selain itu, manajemen backpressure untuk menghindari kelebihan beban pada client atau server menjadi penting dalam situasi dengan volume pesan tinggi guna menjaga stabilitas sistem. Error-handling juga menjadi tantangan karena kompleksitas dalam menyampaikan dan error-handling dalam bidirectional streaming lebih tinggi dibandingkan dengan RPC unary atau server streaming. Selanjutnya, proses pengujian dan debugging menjadi lebih menantang karena sifat komunikasi concurrent.

### 4.  What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?
> **Advantages**
> 
> -   **Decoupling**: `ReceiverStream` melepaskan ketergantungan antara server dan client, memungkinkan kode yang lebih fleksibel dan modular.
> -   **Asynchronous Processing**: `ReceiverStream` terintegrasi dengan baik dengan paradigma async/await dan runtime Tokio, memungkinkan pemrosesan data streaming secara asynchronous yang efisien.
> -   **Backpressure Handling**: `ReceiverStream` dapat membantu dengan manajemen backpressure, karena server dapat menunggu client untuk menerima pesan sebelum mengirimkan data lebih banyak.
> 
> **Disadvantages**
> -   **Complexity**: Menggunakan `ReceiverStream` menambahkan kompleksitas pada kode, karena memerlukan pengelolaan saluran dan koordinasi antara tugas server dan client.
> -   **Overhead**: Overhead yang terkait dengan menggunakan saluran dan perpindahan pesan dapat berdampak pada kinerja dalam skenario tertentu.

### 5.  In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?
> Strategi pengembangan efektif dengan Rust gRPC mencakup modularisasi dan penggunaan dependency injection untuk memisahkan implementasi layanan dari definisi Protocol Buffer serta dependency  eksternal seperti database. Selain itu, pemanfaatan Traits memungkinkan abstraction antara services dan concrete implementations, memisahkan business logic dari infrastruktur gRPC.

### 6.  In the **MyPaymentService** implementation, what additional steps might be necessary to handle more complex payment processing logic?
> 1.  Validasi request memastikan informasi yang diperlukan ada dan valid
> 2.  Error-handling untuk menangani error yang mungkin terjadi.
> 3.  Interaksi dengan database digunakan untuk memeriksa saldo pengguna, memperbarui saldo setelah pembayaran, dan mencatat transaksi tersebut.
>
### 7.  What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?
> gRPC memfasilitasi interoperabilitas antar bahasa pemrograman dan platform dengan Protocol Buffers sebagai Interface Definition Language (IDL) yang tidak tergantung pada bahasa tertentu. Dukungan streaming bidirectional memungkinkan komunikasi real-time, sementara pengembangan berbasis kontrak dengan Protocol Buffers memastikan konsistensi antar layanan. 
> 
### 8.  What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?
> Kelebihan menggunakan HTTP/2 meliputi kemampuan multiplexing yang memungkinkan penanganan multiple request dan response secara bersamaan dalam satu koneksi TCP, header compression yang mengurangi overhead terutama untuk request dan response dengan header besar atau berulang, serta kemampuan server push yang meningkatkan kinerja dengan mengirimkan sumber daya ke klien secara proaktif. 
> 
> Kekurangan HTTP/2 meliputi kompleksitas implementasi dan debugging yang lebih tinggi dibandingkan dengan HTTP/1.1, serta penggunaan lebih banyak sumber daya server seperti CPU dan memori karena overhead multiplexing dan header compression. Selain itu, strategi caching perantara dapat menjadi lebih sulit dengan penggunaan multiplexing dan server push oleh HTTP/2, mengurangi efektivitas caching dibandingkan dengan HTTP/1.1.
> 
### 9.  How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?
> Pendekatan komunikasi real-time dan responsif dalam REST API, yang mengikuti model response-request, berbeda dengan kemampuan streaming bidirectional gRPC. REST API mengharuskan client mengirim request ke server dan menunggu response dengan data yang diminta, sementara gRPC mendukung streaming bidirectional, memungkinkan pertukaran pesan asynchronous antara client dan server melalui satu koneksi, memfasilitasi komunikasi real-time tanpa menunggu request atau response.
> 
### 10.  What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?
> Dalam hal penanganan payload, pendekatan gRPC yang berbasis skema dengan Protocol Buffers memeberikan validasi data yang ketat, otomatisasi kode, dan konsistensi yang kuat antara client dan server. Di sisi lain, penggunaan JSON dalam REST API memberikan fleksibilitas lebih besar dalam penanganan data yang dinamis dan perubahan struktur tanpa perlu memperbarui skema atau kode. Pemilihan antara keduanya tergantung pada kebutuhan spesifik dari setiap kasus penggunaan.
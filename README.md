### Reflection Modul 9 
1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?
- Unary: satu permintaan dari klien dan satu respon dari server. Unary cocok digunakan ketika klien hanya meminta satu data atau melakukan satu operasi, seperti mengambil suatu data profil klien.
- Server streaming: satu permintaan dari klien dan aliran respons dari server, memungkinkan client-side untuk memulai processing data sebelum transfer end. Server streaming cocok digunakan ketika klien meminta data yang besar, atau data set yang perlu diproses seiring dengan datangnya data.
- bi-directional streaming RPC: klien dan server berkomunikasi dengan aliran data secara independen secara bersamaan. Cocok digunakan untuk tipe data yang iteratif dan memerlukan interaksi langsung antara klien dan server, seperti chat.

2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?

- Autentikasi: gRPC mendukung beberapa metode autentikasi, seperti SSL/TLS, OAuth2, dan token-based authentication. Untuk mengamankan layanan gRPC, kita dapat menggunakan SSL/TLS untuk mengenkripsi data yang dikirim antara klien dan server.

- Otorisasi: gRPC vulnerable kepada BOLA (Broken Object Level Authorization) attack, dimana attacker dapat mengakses data yang tidak seharusnya. Untuk mengatasi hal ini, kita dapat menggunakan middleware atau interceptor untuk melakukan validasi otorisasi sebelum request diteruskan ke handler.

- Enkripsi data: gRPC mendukung enkripsi data menggunakan TLS. Dengan menggunakan enkripsi data, kita dapat mencegah data yang dikirim antara klien dan server dari disadap oleh pihak yang tidak berwenang.

3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?

Bidirectional streaming memungkinkan klien dan server untuk berkomunikasi secara independen secara bersamaan, ini berarti kita perlu menangani banyak proses yang berkerja secara conccurent dan biasanya asinkron, ini memerlukan pemahaman kompleksitas pembagian sumber daya dan sinkronisasi data. Data serialization dan deserialization juga menjadi tantangan, karena kita perlu memastikan data yang dikirim dan diterima oleh klien dan server sesuai dengan protokol yang telah ditentukan. 

4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?

+:
1) Integrasi dengan Tokio
2) Simplifikasi handling stream secara asinkron

-:
1) Coupling dengan Tokio yang sangat ketat
2) Kontrol terbatas threading dan blocking
3) learning curve, susah untuk diimplementasikan bagi pengguna baru

5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

Menggunakan file proto untuk perubahan dan penambahan fitur mwmbuat kode lebih mudah dimaintaian dan diextend.

6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

"kompleks" dalam konteks ini bisa berarti banyak hal, namun beberapa hal yang mungkin dapat ditambahkan adalah seperti membuat proses pembayaran menggunakan server streaming, berguna jika klien ingin real-time update atau jika proses pembayaran yang lebih kompleks memerlukan waktu yang lama untuk diproses.

7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

gRPC memungkinkan developer untuk berkerja dengan berbagai language berbeda, dan membuat building di berbagai sistem lebih mudah dengan protocol buffer berkerja sebagai skema data yang netral bahasa. Code generation memungkinkan kita untuk mengskip boilerplate code dan fokus pada bisnis logic pada service service yang kita buat.

8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

+:
1) Multiplexing: HTTP/2 mendukung multiplexing, memungkinkan klien dan server untuk mengirim beberapa request dan response secara bersamaan.
2) binary protocol: HTTP/2 menggunakan binary protocol, yang lebih efisien dalam parsing dan serialization data.
3) Header compression: HTTP/2 menggunakan HPACK untuk mengompresi header, mengurangi overhead data yang dikirim antara klien dan server.
4) Stream prioritization: HTTP/2 mendukung stream prioritization, memungkinkan klien untuk menentukan prioritas request yang dikirim ke server.

-:
1) Security concern dan cost: HTTP/2 membutuhkan SSL/TLS untuk enkripsi data, yang memerlukan biaya tambahan untuk setup dan maintenance. Walaupun tidak wajib, ini bisa mempengaruhi keamanan dan performa.
2) Resource intensive: HTTP/2 membutuhkan lebih banyak resource untuk parsing dan handling data, dibandingkan dengan HTTP/1.1.


9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

REST API menggunakan model request-response yang secara ekslusif merupakan unary cycle, dimana setiap request dari klien akan direspon satu-persatu oleh server. REST API juga menggunakan method HTTP untuk setiap akses sumberdaya dengan endpoint yang kita buat.

10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

Yang jelas dengan schema adalah kemungkinan untuk strict typing pada setiap data yang diinginkan server. ini bisa menjadi keuntungan dan kerugian. Keuntungan adalah kita bisa memastikan data yang dikirim dan diterima sesuai dengan yang diinginkan, namun kerugian adalah jika ada perubahan pada schema, maka kita harus melakukan update pada semua service yang terkait. JSON lebih fleksibel, namun bisa menyebabkan data yang tidak sesuai dengan yang diinginkan server. perlu diingat juga bahwa semua data di gRPC diproses secara automatis oleh protobuf, sehingga bisa mempercepat proses parsing dan serialization data.


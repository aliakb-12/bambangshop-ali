# BambangShop Publisher App
Tutorial and Example for Advanced Programming 2024 - Faculty of Computer Science, Universitas Indonesia

---

## About this Project
In this repository, we have provided you a REST (REpresentational State Transfer) API project using Rocket web framework.

This project consists of four modules:
1.  `controller`: this module contains handler functions used to receive request and send responses.
    In Model-View-Controller (MVC) pattern, this is the Controller part.
2.  `model`: this module contains structs that serve as data containers.
    In MVC pattern, this is the Model part.
3.  `service`: this module contains structs with business logic methods.
    In MVC pattern, this is also the Model part.
4.  `repository`: this module contains structs that serve as databases and methods to access the databases.
    You can use methods of the struct to get list of objects, or operating an object (create, read, update, delete).

This repository provides a basic functionality that makes BambangShop work: ability to create, read, and delete `Product`s.
This repository already contains a functioning `Product` model, repository, service, and controllers that you can try right away.

As this is an Observer Design Pattern tutorial repository, you need to implement another feature: `Notification`.
This feature will notify creation, promotion, and deletion of a product, to external subscribers that are interested of a certain product type.
The subscribers are another Rocket instances, so the notification will be sent using HTTP POST request to each subscriber's `receive notification` address.

## API Documentations

You can download the Postman Collection JSON here: https://ristek.link/AdvProgWeek7Postman

After you download the Postman Collection, you can try the endpoints inside "BambangShop Publisher" folder.
This Postman collection also contains endpoints that you need to implement later on (the `Notification` feature).

Postman is an installable client that you can use to test web endpoints using HTTP request.
You can also make automated functional testing scripts for REST API projects using this client.
You can install Postman via this website: https://www.postman.com/downloads/

## How to Run in Development Environment
1.  Set up environment variables first by creating `.env` file.
    Here is the example of `.env` file:
    ```bash
    APP_INSTANCE_ROOT_URL="http://localhost:8000"
    ```
    Here are the details of each environment variable:
    | variable              | type   | description                                                |
    |-----------------------|--------|------------------------------------------------------------|
    | APP_INSTANCE_ROOT_URL | string | URL address where this publisher instance can be accessed. |
2.  Use `cargo run` to run this app.
    (You might want to use `cargo check` if you only need to verify your work without running the app.)

## Mandatory Checklists (Publisher)
-   [ ] Clone https://gitlab.com/ichlaffterlalu/bambangshop to a new repository.
-   **STAGE 1: Implement models and repositories**
    -   [ ] Commit: `Create Subscriber model struct.`
    -   [ ] Commit: `Create Notification model struct.`
    -   [ ] Commit: `Create Subscriber database and Subscriber repository struct skeleton.`
    -   [ ] Commit: `Implement add function in Subscriber repository.`
    -   [ ] Commit: `Implement list_all function in Subscriber repository.`
    -   [ ] Commit: `Implement delete function in Subscriber repository.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-1" questions in this README.
-   **STAGE 2: Implement services and controllers**
    -   [ ] Commit: `Create Notification service struct skeleton.`
    -   [ ] Commit: `Implement subscribe function in Notification service.`
    -   [ ] Commit: `Implement subscribe function in Notification controller.`
    -   [ ] Commit: `Implement unsubscribe function in Notification service.`
    -   [ ] Commit: `Implement unsubscribe function in Notification controller.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-2" questions in this README.
-   **STAGE 3: Implement notification mechanism**
    -   [ ] Commit: `Implement update method in Subscriber model to send notification HTTP requests.`
    -   [ ] Commit: `Implement notify function in Notification service to notify each Subscriber.`
    -   [ ] Commit: `Implement publish function in Program service and Program controller.`
    -   [ ] Commit: `Edit Product service methods to call notify after create/delete.`
    -   [ ] Write answers of your learning module's "Reflection Publisher-3" questions in this README.

## Your Reflections
This is the place for you to write reflections:

### Mandatory (Publisher) Reflections

#### Reflection Publisher-1
PERTANYAAN 1
satu Model Struct saja sudah cukup karena semua subscriber memiliki perilaku yang sama, yaitu hanya menerima data url. Interface
biasanya digunakan jika kita mempunyai subcriber yang memupnyai cara kerja yang berbeda-beda (seperti ada yang lewat email, sms, dll).
Karena di kode ini variasinya cuma data, penggunaan trait di rust akan membuat kode lebih complex tanpa ada manfaat nyatanya.

PERTANYAAN 2
Penggunaan Dashmmap jauh lebih baik daripada vec(list). Dengan dashmap (Map) kita akan langsung mencari, memperbarui, atau menghapus data secara instan menggunakan kuncinya (O(1) complexity). Sementara pada vec, kita harus menelusuri data satu-satu dari awal sampai akhir (O(N) complexity). 

PERTANYAAN 3:
Singleton (melalui lazy_static!) memastikan hanya ada satu tempat penyimpanan data di seluruh aplikasi, sementara DashMap memastikan tempat penyimpanan tersebut aman saat diakses banyak thread secara bersamaan. Di Rust, aturan borrow checker membuat kita tidak bisa hanya mengandalkan Singleton dengan HashMap biasa karena akan terjadi data race saat banyak user mengakses data tersebut sekaligus. Oleh karena itu, DashMap tetap diperlukan sebagai mekanisme pengunci (locking) internal agar aplikasi tidak crash atau korupsi data. 

#### Reflection Publisher-2
PERTANYAAN 1 : why we need to separate “Service” and “Repository” from a Model?
Pemisahan Service dan Repository dari Model diperlukan untuk menerapkan prinsip desain seperti Single Responsibility Principle (SRP), karena jika Model menangani sekaligus logika bisnis dan akses data, maka model akan menjadi terlalu kompleks dan sulit dikelola. dengan memindahkan logika bisnis ke Service, kita mendapatkan lapisan yang fokus pada aturan dan proses aplikasi, sementara Repository khusus menangani interaksi dengan database atau penyimpanan data, sehingga kode menjadi lebih modular dan perubahan pada satu bagian (misalnya database) tidak memengaruhi logika bisnis secara langsung.

PERTANYAAN 2 : What happens if we only use the Model?
Jika hanya menggunakan Model tanpa pemisahan Service dan Repository, maka setiap model seperti Program, Subscriber, dan Notification akan saling bergantung dan harus menangani banyak tanggung jawab sekaligus (logika bisnis + akses data + koordinasi antar model), sehingga interaksinya menjadi sangat kompleks, misalnya Notification harus langsung mengakses Subscriber untuk mengirim notifikasi, sementara Subscriber juga mungkin perlu mengetahui Program tertentu, sehingga perubahan kecil pada satu model bisa berdampak ke banyak model lain, kode menjadi sulit dibaca, sulit diuji, dan berisiko tinggi terhadap bug karena logika tersebar dan tidak terstruktur dengan jelas

PERTANYAAN 3: Have you explored more about Postman? 
Postman sangat membantu dalam menguji pekerjaan saya karena memungkinkan kita untuk mengirim HTTP request (GET, POST, dll.) langsung ke API yang sedang dikembangkan tanpa perlu membuat frontend terlebih dahulu, sehingga bisa dengan cepat memverifikasi apakah endpoint seperti subscribe dan unsubscribe sudah berjalan dengan benar, termasuk mengecek response, status code, dan error handling


#### Reflection Publisher-3

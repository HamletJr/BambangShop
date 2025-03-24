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
1. Menurut saya pada kasus ini, *interface* atau *trait* belum diperlukan karena hanya terdapat satu *class* saja yang menjadi *subscriber*, namun ke depannya jika ada class lain yang ingin menjadi *subscriber* sebaiknya dibuat interface untuk menjaga prinsip OCP. 
2. Menurut saya `DashMap` bisa saja tidak digunakan, tetapi hal tersebut akan mempersulit implementasi karena kita perlu menggunakan 2 `Vec` (list) untuk menyimpan produk dan *subscriber*-nya.
3. Menurut saya Singleton *pattern* tetap perlu digunakan bersama dengan `DashMap`, karena kita ingin agar hanya ada satu daftar `SUBSCRIBERS` yang digunakan, tetapi `SUBSCRIBERS` tersebut harus dapat digunakan oleh banyak *thread* secara bersamaan sehingga diperlukan penggunaan `DashMap` agar `SUBSCRIBERS` dapat diakses secara *thread-safe*.

#### Reflection Publisher-2
1. Jika Model bertanggung jawab atas *data storage* dan *business logic* sekaligus, itu berarti Model melanggar prinsip SRP (Single Responsibility Principle). Oleh karena itu, Model perlu dipisah menjadi Service dan Repository agar setiap komponen bertanggung jawab atas salah satu dari *data storage* atau *business logic*. Hal ini akan mendukung *clean code* dan meningkatkan *maintainability* dari *codebase* kita.
2. Kalau kita menggunakan Model tanpa separasi, maka kita akan mendapatkan bahwa kode kita menjadi panjang sekali dan kita akan sulit untuk melakukan *maintenance* pada masing-masing model seperti Program, Subscriber, dan Notification. Selain itu, setiap model akan saling bergantung satu sama lain sehingga jika ada perubahan pada salah satu model, bisa jadi model yang lain harus diubah juga.
3. Postman sangat berguna untuk menguji *endpoint* yang kita membuat dengan mensimulasikan request HTTP seperti GET, POST, DELETE, PUT, PATCH, dan lain-lainnya. Kita juga bisa mengatur isi dari request yang dikirim, seperti *request header* dan *request body* (*form data*, *raw data*, JSON, dan lain-lain). Setelah itu, kita bisa melihat response dari aplikasi kita untuk menentukan apakah sudah sesuai dengan ekspektasi kita.

#### Reflection Publisher-3
1. Pada tutorial ini, variasi Observer yang digunakan adalah *push model*, karena setiap kali terjadi perubahan pada produk, *publisher* akan *notify* semua *subscriber* (*push data*), sedangkan *subscriber* tidak dapat meminta data dari *publisher* (*pull data*).
2. Kalau kita menggunakan *pull model*, maka kelebihannya adalah *subscriber* dapat memilih kapan dia perlu meminta data dari *publisher*, jadi *publisher* tidak perlu boros mengirim data kepada *subscriber* jika mereka tidak membutuhkannya saat itu. Namun, kelemahan dari approach ini adalah *subscriber* perlu mengetahui data seperti apa yang akan dikirim oleh *publisher* sehingga menambah *coupling* atau *dependency* antara *publisher* dan *subscriber*.
3. Jika aplikasi kita tidak menggunakan *multi-threading*, maka bisa jadi ada banyak *subscriber* yang perlu di-*notify* oleh *publisher*, dan karena hanya ada satu *thread* maka setiap *subscriber* harus di-*update* satu per satu sehingga dapat membuat proses *update* ini mahal dan lambat. Karena kita menggunakan *multi-threading*, aplikasi kita dapat meng-*update* lebih banyak *subscriber* pada satu waktu sehingga mengurangi kemungkinan terjadinya penumpukan atau antrian yang panjang.
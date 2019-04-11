# Orchestration using Docker Compose
---
Semua konfigurasi untuk membuat docker compose berada pada file docker-compose.yml. Semua perintah yang ada pada saat menjalankan
container `docker run` dapat di definisikan di file tersebut. Format file tersebut berdasarakan YAML (YEt Another Markup Language)
Format penulisan kurang lebih sebagai berikut :
```
container_name:
  property: value
    - or options
```
Dalam katakoda, terdapat skenario dimana kita memiliki aplikasi node.js yang memerlukan koneksi ke redis. Untuk memulainya pertama,
seting terlebih dahulu file `docker-compose.yml`. Definisikan container yang akan dibuat, disini untuk node.js digunakan container
bernama web yang akan mem-build container berdasarkan direktorinya. Agar lebih paham, maka membuat file `docker-compose.yml` ini
secara step-by-step. Pertama, definisikan container web dan perintah build untuk menjalankan segala properti yang ada dalam folder
tersebut. 
Identasi tepat dibawah peritah `build`, tambahkan perintah :
```
links:
    - redis
```
Perintah diatas untuk menghubungkan antara dua kontainer agar dapat saling berinteraksi satu sama lain. Pada contoh diatas,
menghubungkan antara kontainer web (aplikasi node.js) dengan kontainer database redis.
Tambahkan properti yang lain, yaitu port indentasi persis dibawah perintah `links`:
```
 ports:
    - "3000"
    - "8000"
```
Selanjutnya mendefiniskan container kedua yaitu redis yang akan disambungkan dengan container web (aplikasi node.js) :
```
redis:
  image: redis:alpine
  volumes:
    - /var/redis/data:/data
```
Untuk definisi container baru, maka indentasi sejajar dengan perintah `web`. Untuk mendefinisikan container baru tinggal
menuliskan di line baru di file yang sama. Mendefinisikan container kedua, berbeda dengan mendefinisikan container pada
container pertama sebab di container pertama membuat langsung dari current direktori yaitu memanfaatkan `Dockerfile` &
'Makefile'. Untuk container kedua ini, dibuat berdasarkan image yang sudah tersedia di docker hub.
Setelah file `docker-compose.yml` sudah terseting dengan baik, lalu jalankan docker-compose dengan perintah :
```
docker-compose up -d
```
Perintah diatas akan menjalankan docker-compose secara daemon (latar belakang) dengan option -d. Maka kedua container yang
di define sebelumnya akan berjalan secara bersamaan.
Untuk mengeceknya menggunakan perintah :
```
docker-compose ps
```
Maka akan muncul semua container yang berjalan menggunakan docker-compose tadi.
Untuk melihat semua log, menggunakan perintah :
```
docker-compose logs
```
Untuk melihat semua perintah yang ada pada docker-compose, gunakan :
```
docker-compose
```
Selanjutnya docker-compose dapat juga untuk me-manage berapa container yang dijalankan dengan perintah `scale`. Diikuti dengan nama container yang akan di scale serta jumlah containernya. Jika jumlahnya lebih besar dari jumlah container yang sedang berjalan maka akan menjalankan container yang sama sebanyak jumlah container yang di definisikan. Jika jumlahnya lebih kecil dari jumlah container yang sedang berjalan, maka container yang sedang berjalan tersebut akan di kurangi jumlahnya sesuai jumlah container yang di definisikan. Contoh perintah :
```
docker-compose scale web=3
```
Maka akan menjalankan 3 container web secara bersamaan. Jika dijalankan perintah :
```
docker-compose scale web=1
```
Maka akan kembali menjalankan 1 container web saja. Cek dengan perintah :
```
docker-compose ps
```

Selanjutnya, untuk menghentikan container ketikkan perintah :
```
docker-compose stop
```
Maka container akan berhenti, cek statusnya dengan `docker-compose ps` terlihat status menjadi `exited`.
Selanjutnya untuk menghapus container-container yang ada, ketikkan perintah :
```
docker-compose rm
```
Cek dengan `docker-compose ps`, maka container yang sebelumnya ada dilist akan hilang dari list.



# Docker Orchestration - Getting Started With Swarm Mode
---

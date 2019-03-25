# Docker Katacoda Tutorial
___

## Deploying Your First Docker Container

Pada tutorial ini, belajar menjalankan suatu container yaitu redis.
Pertama, cari dulu di repository dengan nama redis :
```
docker search redis
```
Setelah tau nama repository nya, pull repository redis :
```
docker pull redis
```
Karena images redis sudah di download dari dockerhub, jalankan image tersebut
agar menjadi container. Karena redis adalah sebuah database, maka dijalankan secara continue. Sehingga menggunakan option -d
agar berjalan secara daemon/continue. Perintahnya :
```
docker run -d redis
```
Dengan perintah diatas docker akan menjalankan versi redis terbaru. Untuk memakai versi redis tertentu maka bisa menambahkan
versinya, menjadi seperti berikut :
```
docker run -d redis:3.2
```
Redis telah dijalankan sebagai container, untuk mengecek list container yang sedang berjalan gunakan perintah :
```
docker ps
```
atau
```
docker container ls
```
Untuk melihat container yang sudah dibuat, entah itu yang berjalan atau pun tidak :
```
docker ps -a
```
atau
```
docker container ls -a
```
Setelah perintah diatas dijalankan maka akan muncul list container yang sedang berjalan. Disitu akan terlihat informasi 
CONTAINER ID, IMAGE, STATUS, dan lain sebagainya.
Redis sudah berjalan agar redis bisa dikases diluar docker maka kita 
perlu mengekspos service docker melalui port tertentu. Maka gunakan perintah :
```
docker run -d --name redisHostPort -p 6379:63779 redis:latest
```
Dimana arti -d adalah untuk menjalankan container secara daemon (latar belakang). Lalu --name untuk memberi nama container yang berjalan, pada contoh tersebut 'redisHostPort'. Serta -p untuk mendefinisikan port dari container tersebut. Secara default port local adalah 0.0.0.0.

Permasalahan dari port yang di definisikan terlebih dahulu secara spesifik adalah hanya bisa menjalankan satu instance aja. Jika hanya ingin menjalankan port untuk beberapa instance gunakan :
```
docker run -d --name redisDynamic -p 6379 redis:latest
```
Untuk mengetahui port yang sedang berjalan :
```
docker port redisDynamic 6379
```
atau
```
docker ps
```

Secara default data yang disimpan melalui docker akan hilang jika container di hapus atau pun di buat ulang. Agar data tidak hilang saaat container dihapus atau di buat ulang maka harus disimpan disebuah directory. Data-data tersebut disimpan di /opt/docker/data/redis. Dengan perintah :
```
docker run -d --name redisMapped -v /opt/docker/data/redis:/data redis
```
Disini -v mendeskripsikan folder yang digunakan untuk menyimpan data.

Perintah :
```
docker run ubuntu ps
```
Digunakan untuk menjalankan container ubuntu, sekaligus melihat proses apa saja yang berjalan pada container tersebut.
Untuk mengakses bash shell pada container ubuntu, gunakan :
```
docker run -it ubuntu bash
```

## Deploy Static HTML Website as Container




## Building Container Image



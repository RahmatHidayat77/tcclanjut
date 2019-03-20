# Docker Katacoda Tutorial
___

## Deploying Your First Docker Container

Pada tutorial ini yaitu bagaimana menjalankan 
Karena images redis sudah di download dari dockerhub, maka langkah selalnjutnya adalah menjalankan image tersebut
agar menjadi conatiner. Karena redis adalah sebuah databse, maka dijalankan secara continue. Sehingga menggunakan option -d
yaitu agar berjalan secara daemon/continue. Perintahnya :
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
sama dengan 
```
docker container ls
```
Setelah perintah diatas dijalankan maka akan muncul list container yang sedang berjalan. Disitu akan terlihat informasi 
CONTAINER ID, IMAGE, STATUS, dan lali sebagainya. Redis sudah berjalan agar redis bisa dikases diluar docker maka kita 
perlu mengekspos service docker melalui host. 

## Deploy Static HTML Website as Container

## Building Container Image

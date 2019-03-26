# Docker Katacoda Tutorial
___

## Deploying Your First Docker Container
___

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
___
Di tutorial ini, belajar membuat docker image yang menjalankan HTML static menggunakan nginx. Pertama, buat Dockerfile yaitu base image yang digunakan untuk membuat docker image. Dalam tutorial tersebut menggunakan webserver nginx yang ada pada linux alpine. Jadi base imagenya adalah linux alpine dengan webserver nginx. Contoh docker file :
```
FROM nginx:alpine
COPY . /usr/share/nginx/html
```
Simpan file diatas dengan nama: Dockerfile. Setelah disimpan, kita tinggal build dockerfile tersebut dengan perintah :
```
docker build -t webserver-image:v1 .
```
Dengan catatan, build dockerfile harus di directory dockerfile tersebut. Option -t untuk menamai image dan memberi tag pada image. Contoh diatas nama image adalah 'webserver-image', dengan tag 'v1'.
Untuk mengetahui list semua images yang ada di komputer :
```
docker images
```
Setelah images jadi, run image sebagai container :
```
docker run -d -p 80:80 webserver-image:v1
```
Sesuai perintah diatas, aplikasi berjalan melalui port local 80 dan port docker 80. Untuk cek container yang sedang
berjalan :
```
docker ps
```
Untuk cek aplikasi bisa di akses atau tidak :
```
curl docker
```


## Building Container Image
___
Di tutorial ini, lebih menjelaskan bagaimana membuat Dockerfile dari awal. Lalu mem-build nya menjadi sebuah docker image, sampai mendeploy menjadi sebuah container yang bisa diakses dari luar docker.
Dalam scenario ini, yaitu membuat container nginx yang dapat menjalankan static HTML. Langkah pertama yang harus dilakukan 
adalah membuat Dockerfile terlebih dahulu :
```
FROM nginx:1.11-alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```
Simpan file diatas dengan nama Dockerfile, simpan di directory yang sudah ada file html untuk nantinya dicopy ke dalam
dockerfile.
Penjelasan dari file diatas :
Line 1, adalah base image yang digunakan. Contoh diatas menggunakan nginx:1.11-alpine, base image adalah image dasar untuk menjalankan aplikasi tersebut. Dimana sebelum menjalankan file html, perlu untuk menjalankan nginx terlebih dahulu.
Line 2, mengcopy kan file index.html ke diretory yang ada pada dockerfile. Agar nantinya bisa ditampilkan di browser.
Line 3, perintah expose berfungsi mendefinisikan port yang digunakan untuk mengakses aplikasi docker tersebut.
Line 4, perintah CMD mendefinisikan perintah default yang akan dieksekusi saat doker container di jalankan. 

Setelah Dockerfile selesai dibuat, langkah selanjutnya build Dockerfile agar menjadi docker image. Perintahnya sebagai berikut :
```
docker build -t my-nginx-image:latest .
```
Optioin -t untuk memberi nama docker image serta memberi tag yang mengarah ke versi image tersebut.

Cek di list, image sudah ada di list atau belum :
```
docker images
```
Selanjutnya deploy image di container dengan perintah :
```
docker run -d -p 80:80 my-nginx-image:latest
```
Penjelasan, option -d agar aplikasi berjalan secara daemon (background), -p mendefinisikan port yang digunakan. Kiri adalah
port local dan kanan adalah port docker. diikuti nama images dan tag. Setelah itu bisa dicek apakah container sudah berjalan attu belum, dengan perintah :
```
docker ps
```
Akan menampilkan list container yang sedang berjalan. Untuk mengakses apakah nginx sudah berjalan atau belum. Ketik kan perintah :
```
curl -i http://docker
```
Cek apakah sesuai, dengan yang ada pada file index.html. Atau bisa juga dengan membuka browser, lalu ketik kan url localhost. Lihat apakah nginx sudah berjalan atau belum. Dengan cacatan stop dahulu webserver yang ada di local komputer, contohnya apache2.

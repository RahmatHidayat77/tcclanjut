# Katakoda Docker Tutorial
___

## Docker Network
___

Tutorial ini menjelaskan bagaimana membuat network untuk docker container. Sehingga container satu dengan container yang lain dapat
saling berhubungan. Sebelumnya, untuk membuat jaringan untuk docker container ada dua pendekatan yaitu link dan network. Di tutorial
akan menggunakan network.
Pertama, buat network beserta nama network. Contoh :
```
docker network create backend-network
```
Network sudah terbuat, bisa di cek menggunakan :
```
docker network ls
```
Maka akan muncul list network yang tersedia, termasuk network yang baru saja dibuat.

Selanjutnya untuk menspesifikasi container agar menggunakan network tertentu. Bisa di define saat membuat container. Contoh :
```
docker run -d --name=redis --net=backend-network redis
```
Perintah di atas membuat container redis menggunakan network: backend-network. Option --net mendefinisikan network yang digunakan.
Untuk mengetest koneksi antara dua container, gunakan perintah :
```
docker run --net=backend-network alpine ping -c1 redis
```
Dimana, network yang digunakan adalah backend-network. Container yang di test adalah alpine mengakses jaringan ke redis. Dengan
list satu line saja, -c1 mendefinisikan jumlah test yang dilakukan.

Dalam satu container docker memungkinkan dapat memiliki lebih dari satu network. Contoh, buat network baru :
```
docker network create frontend-network
```
Cek network baru di list :
```
docker network ls
```
Lalu tambahkan network baru ke dalam container redis :
```
docker network connect frontend-network redis
```

Kemudian buat container baru yang menggunakan network yang sama :
```
docker run -d -p 3000:3000 --net=frontend-network katacoda/redis-node-docker-example
```

Cek jaringan keduanya :
```
curl docker:3000
```

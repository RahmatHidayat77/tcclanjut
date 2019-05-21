# GO LANGUAGE

## Hello World
Go merupakan bahasa pemograman yang sudah terdapat webserver di dalamnya (built-in). Package net/http merupakan standart library yang
sudah terdapat semua fungsionalitas protocol HTTP.

Registrasi request handler :
Buat handler yang dapat menerima semua koneksi HTTP dari browser, HTTP client, atau API request. Bentuk handler pada go,
```
func (w http.ResponseWriter, r *http.Request)
```
function diatas menerima dua parameter, yaitu :
- `http.ResponseWriter` digunakan untuk mengedit response text/html.
- `http.Request` berisi emua informasi tentang HTTP request tersebut seperti URL atau fields header.

Contoh meregristasi request handler ke default HTTP Server,
```
http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
	fmt.Fprintf(w, "Hello, you've requested: %s\n", r.URL.Path)
})
```

Listen ke sambungan HTTP :
Request handler tidak dapat diterima oleh koneksi HTTP dari luar. Sebuah HTTP server harus mengarah ke suatu port untuk terkoneksi ke
request handler. Port 80 adalah port default untuk sambungan HTTP, server tersebut juga akan mengarah ke port 80.
Code dibawah akan menjalankan default HTTP server dan akan mengarah ke port 80. Setelah itu kita dapat mengakses ke `http://localhost/`
di browser untuk mengaksesnya.
```
http.listenAndServe(":80",nil)
```

Kode keseluruhan dari praktik diatas :
```
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Hello, you've requested: %s\n", r.URL.Path)
	})

	http.ListenAndServe(":80", nil)
}

```

---

## HTTP Server
Basic HTTP server mempunyai beberapa tugas yaitu,
* Memproses request yang dinamis : memproses request yang masuk dari user yang mengakakses website, masuk ke akun miliknya, atau mengirimkan gambar.
* Menjalankan aset statis : menjalankan Javascript, CSS, dan gambar pada browser untuk membuat pengalaman yang dinamis kepada user.
* Menerima koneksi : HTTP server harus mengarah pada sebuah port yang spesifik untuk dapat menerima koneksi dari internet.

Proses request dinamis :
Package `net/http` memiliki semua fungsi yang dibutuhkan untuk menerima request dan menghandle nya secara dinamis. Kita dapat me-register
handler baru menggunakan fungsi `http.HandleFunc`. Parameter pertama merujuk ke path yang dituju, sedangkan paramater kedua merujuk
fungsi yang harus di eksekusi.
Contoh untuk menampilkan "Welcome to my website" :
```
http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
	fmt.Fprint(w, "Welcome to my website!")
})
```

`http.Request` terdiri dari semua informasi tentang request dan parameternya. Kita dapat melihat parameter GET menggunakan 
`r.URL.Query().Get("token")` atau parameter POST (field dari form HTML) menggunakan `r.FormValue("email")`.

Menghandle asset static :
Untuk menghandle asset static seperti javascript, CSS, dan gambar, kita dapat menggunakan `http.FileServer` dan mengarahkannya ke sebuah path url. Agar file server bekerja dengan baik, maka harus tahu darimana asal file tersebut. Contoh :
```
fs := http.FileServer(http.Dir("static/"))
```
Sekali file server kita berada dalam direktori tersebut maka kita tinggal mengarahkan url path kek direktori tersebut, sama seperti yang kita lakukan pada dinamik request. Catatan, urutan untuk menghandle file secara benar kita perlu menghilangkan bagian yang tidak perlu pada url path. Biasanya merupakan nama direktori dimana file tersebut kita tempatkan. Contoh :
```
http.Handle("/static/", http.StripPrefix("/static/", fs))
```

Menerima koneksi :
Satu hal yang harus dilakukan untuk menyelesaikan HTTP server basic, yaitu kita arahkan HTTP derver tersebut ke sebuah port untuk menerima sambungan dari internet. Seperti yang sudah dibahas sebelumnya, GO memiliki built-in HTTP server. Maka kita dapat menggunakannya. Contoh :
```
http.ListenAndServe(":80", nil)
```
Untuk mengakses aplikasi dengan port diatas kita tinggal mengisi alamat browser dengan localhost saja, sebab port 80 adalah port default. Contoh kode lengkap, dari penjelasan diatas :
```
package main

import (
	"fmt"
	"net/http"
)

func main() {
	http.HandleFunc("/", func (w http.ResponseWriter, r *http.Request) {
		fmt.Fprintf(w, "Welcome to my website!")
	})

	fs := http.FileServer(http.Dir("static/"))
	http.Handle("/static/", http.StripPrefix("/static/", fs))

	http.ListenAndServe(":80", nil)
}
```

---

## 

## Source : https://gowebexamples.com


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

## Source : https://gowebexamples.com


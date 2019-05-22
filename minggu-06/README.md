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

## Routing menggunakan Gorilla/Mux

Package net/http dari go menyediakan banyak fungsionalitas untuk HTTP protocol. Satu hal yang tidak bisa dilakukan dengan baik dengan net/http adalah melakukan complex request routing seperti membagi sebuah request url menjadi single parameter. Maka dari itu kita dapat menggunakan package gorilla/mux untuk kasus yang kompleks seperti contoh tersebut. 

Install gorilla/mux package :
Packaage gorilla/mux dapat menyesuikan dengan router default HTTP dari Go. Gorilla/mux juga support dengan request handler bawaan Go yaitu `func (w http.ResponseWriter, r *http.Request)`, sehingga gorilla/mux dapat di gabungkan atau dipasangkan dengan library HTTP yang lain seperti middleware atau aplikasi yang sudah ada. Untuk menginstal package gorilla/mux, jalankan perintah berikut.
```
go get -u github.com/gorilla/mux
```

Membuat Router baru :
Pertama buat sebuah router dimana router tersebut nantinya menjadi router utama untuk aplikasi yang kita buat. Yang mana selanjutnya akan menerima parameter dari semua koneksi HTTP dan meneruskannya ke request handler yang akan di daftarkan. Membuat router baru dengan code berikut.
```
r := mux.newRouter()
```

Mendaftarkan sebuah Request Handler :
Setelah kita membuat sebuah router maka kita dapat mendaftarkan requset handler seperti biasanya. Perbedaanya adalah jika sebelumnya kita menggunakan `http.HandleFunc(...)` maka kita dapat menggunakan `r.HandleFunc(...)`, sebab router tersebut sudah tersimpan pada variabel `r`.

URL Parameter :
Kelebihan dari gorilla/mux adalah kemampuan untuk mengesktrak segment dari URL request. Conothnya sebagai berikut.
```
/books/go-programming-blueprint/page/1o
```
URL diatas memiliki dua segmen dinamis.
1. Judul buku (go-progamming-blueprint)
2. Halaman (10)

Agar bisa mendapatkan request handler yang cocok dengan URL diatas kita harus mereplace segmen dinamis dengan placeholder pada pola URL.
```
r.HandleFunc("/books/{title}/page/{page}", func(w http.ResponseWriter, r *http.Request) {
	// get the book
	// navigate to the page
})
```
Terlihat bagian {tittle} dan {page} adalah dinamis dapat di pass bermacam nilai. Selanjutnya, kita dapat mendapatkan data dari segmen tersebut. Yaitu dengan function `mux.Vars(r)` yang mana memakai `http.Request` sebagai parameternya.
```
func(w http.ResponseWriter, r *http.Request) {
	vars := mux.Vars(r)
	vars["title"] // the book title slug
	vars["page"] // the page
}
```

Menyeting router HTTP server :
Pernah kah kita bertanya-tanya, apakah maksud nil pada `http.ListenAndServe(":80", nil)` ? Itu adalah parameter untuk main router dari HTTP server. Sebab defaultnya adalah nil, yang berarti kita menggunakan router default yaitu net/http. Untuk menggunakan router yang kita buat, maka ganti nil dengan variabel dari router yang kita buat yaitu variabel `r`.
```
http.ListenAndServe(":80", r)
```

Berikut code lengkap dari penjelasan diatas.
```
package main

import (
	"fmt"
	"net/http"

	"github.com/gorilla/mux"
)

func main() {
	r := mux.NewRouter()
	r.HandleFunc("/books/{title}/page/{page}", func(w http.ResponseWriter, r *http.Request) {
		vars := mux.Vars(r)
		title := vars["title"]
		page := vars["page"]

		fmt.Fprintf(w, "You've requested the book: %s on page %s\n", title, page)
	})

	http.ListenAndServe(":80", r)
}
```

Fitur dari Router gorilla/mux
Methods
Membatasi request handler untuk method HTTP yang spesifik.
```
r.HandleFunc("/books/{title}", CreateBook).Methods("POST")
r.HandleFunc("/books/{title}", ReadBook).Methods("GET")
r.HandleFunc("/books/{title}", UpdateBook).Methods("PUT")
r.HandleFunc("/books/{title}", DeleteBook).Methods("DELETE")
```

Hostname & Subdomain
Membatasi request handler ke hostname & subdomain.
```
r.HandleFunc("/books/{title}", BookHandler).Host("www.mybookstore.com")
```

Skema
Membatasi reuest hanlder ke http/https
```
r.HandleFunc("/secure", SecureHandler).Schemes("https")
r.HandleFunc("/insecure", InsecureHandler).Schemes("http")
```

Prefik Path & Subrouter
Membatasi reuest handler ke spesifik prefix path.
```
bookrouter := r.PathPrefix("/books").Subrouter()
bookrouter.HandleFunc("/", AllBooks)
bookrouter.HandleFunc("/{title}", GetBook)
```

## Source : https://gowebexamples.com


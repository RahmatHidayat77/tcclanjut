# Cara membuat Pull Request (PR) di Github

Pertama kita pastikan kita punya hak akses untuk memodifikasi file yang ada di dalam repository tersebut atau tidak. 
Jika kita punya hak akses maka kita tinggal clone repo tersebut, lalu mengedit file didalamnya.
Jika tidak, maka kita harus melakukan fork repository terlebih dahulu.
Fork yaitu menyalin repository orang lain ke akun github kita.
Setelah di fork, lalu kita clone repository tersebut. Setelah di clone baru kita edit filenya.

Cara fork :
Klik tombol fork pada sisi kanan atas.

Cara clone:
Copy SSH atau HTTP nya, lalu ketikkan perintah,

```
git clone (alamat SSH atau HTTP)
```

Setelah file di edit, lakukan :

Add file
```
git add (nama file)
```

atau

```
git add .
```

Commit perubahan terbaru
```
git commit -am "(pesan commit)"
```

Push ke branch remote
```
git push -u origin (nama branch)
```

Setelah di push kita harus melakukan "Pull Request" (PR), agar perubahan yang kita push ke remote di merge oleh pemilik repo.
Pemilik repo akan mendapat notifikasi bahwa ada orang lain yang melakukan pull request pada repo miliknya.
Sebelum pemilik repo melakukan merge, dia akan melakukan review terlebih dahulu terhadap code yang di push oleh contributor.
Pemilik repo berhak menyetujui atau menolak pull request tersebut.
Jika pemilik repo menyetujui maka selanjutnya dia akan melakukan merge dengan repo miliknya. Jika tidak maka pull request tersebut di reject oleh pemilik repo.

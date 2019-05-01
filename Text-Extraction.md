# **Text Preprocessing**

adalah tahapan dimana aplikasi melakukan seleksi data yang akan diproses pada setiap dokumen. Proses preprocessing ini meliputi **(1) case folding**, **(2) tokenizing**, **(3) filtering**, dan **(4) stemming**.

[![Material for MkDocs](C:\Users\Choirul\Pictures\pros.jpg)

Dalam pemrograman python, proses text prepocessing dapat dilakukan dengan menggunakan library Sastrawi, library ini merupakan pengembangan dari library PHP Satrawi, dimana library tersebut dikhususkan untuk text berformat bahasa indonesia.

## 1. Tokenisasi

Tahap **Tokenisasi** adalah tahap pemotongan string input berdasarkan tiap kata yang menyusunnya. Contoh dari tahap ini dapat dilihat pada gambar dibawah ini.

Tokenisasi secara garis besar memecah sekumpulan karakter dalam suatu teks ke dalam satuan kata, bagaimana membedakan karakter-karakter tertentu yang dapat diperlakukan sebagai pemisah kata atau bukan.

Sebagai contoh karakter whitespace, seperti enter, tabulasi, spasi dianggap sebagai pemisah kata. Namun untuk karakter petik tunggal **(‘)**, titik **(.)**, semikolon **(;)**, titk dua **(:)** atau lainnya, dapat memiliki peran yang cukup banyak sebagai pemisah kata

[1]: https://informatikalogi.com/text-preprocessing/

## **2. Filtering**

Tahap **Filtering** adalah tahap mengambil kata-kata penting dari hasil token. Bisa menggunakan **algoritma stoplist (membuang kata kurang penting)** atau **wordlist (menyimpan kata penting)**. Stoplist/stopword adalah kata-kata yang tidak deskriptif yang dapat dibuang dalam pendekatan bag-of-words. Contoh stopwords adalah *“yang”*, *“dan”*, *“di”*, *“dari”* dan seterusnya. 

Kata-kata seperti *“dari”*, *“yang”*, *“di”*, dan *“ke”* adalah beberapa contoh kata-kata yang berfrekuensi tinggi dan dapat ditemukan hampir dalam setiap dokumen (disebut sebagai stopword). Penghilangan stopword ini dapat mengurangi ukuran index dan waktu pemrosesan. Selain itu, juga dapat mengurangi level noise.

Namun terkadang stopping tidak selalu meningkatkan nilai retrieval. Pembangunan daftar stopword (disebut stoplist) yang kurang hati-hati dapat memperburuk kinerja sistem **Information Retrieval (IR)**. Belum ada suatu kesimpulan pasti bahwa penggunaan stopping akan selalu meningkatkan nilai retrieval, karena pada beberapa penelitian, hasil yang didapatkan cenderung bervariasi.

## **3. Steaming**

Pembuatan indeks dilakukan karena suatu dokumen tidak dapat dikenali langsung oleh suatu [**Sistem Temu Kembali Informasi**](https://informatikalogi.com/sistem-temu-kembali-informasi/) atau I**nformation Retrieval System (IRS)**. Oleh karena itu, dokumen tersebut terlebih dahulu perlu dipetakan ke dalam suatu representasi dengan menggunakan teks yang berada di dalamnya.

Teknik **Stemming** diperlukan selain untuk memperkecil jumlah indeks yang berbeda dari suatu dokumen, juga untuk melakukan pengelompokan kata-kata lain yang memiliki kata dasar dan arti yang serupa namun memiliki bentuk atau form yang berbeda karena mendapatkan imbuhan yang berbeda.

Sebagai contoh kata bersama, kebersamaan, menyamai, akan distem ke root word-nya yaitu *“sama”*. Namun, seperti halnya stopping, kinerja stemming juga bervariasi dan sering tergantung pada domain bahasa yang digunakan.

Proses stemming pada teks berbahasa Indonesia berbeda dengan stemming pada teks berbahasa Inggris. Pada teks berbahasa Inggris, proses yang diperlukan hanya proses menghilangkan sufiks. Sedangkan pada teks berbahasa Indonesia semua kata imbuhan baik itu sufiks dan prefiks juga dihilangkan.

``` scala
def preprosesing(txt):
    SWfactory = StopWordRemoverFactory()
    stopword = SWfactory.create_stop_word_remover()

    stop = stopword.remove(txt)
    Sfactory = StemmerFactory()
    stemmer = Sfactory.create_stemmer()

    stem = stemmer.stem(stop)
    return stem

def countWord(txt):
    d = dict()
    for i  in txt.split():
        if d.get(i) == None:
            d[i] = txt.count(i)
    return d
```



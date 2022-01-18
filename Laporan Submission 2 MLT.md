# Laporan Machine Learning - Mega Dewi Giridrawardani

## Project Review

Buku seringkali disebut sebagai jendela dunia. Ini berarti dengan membaca buku kita dapat mengetahui serta memahami dunia dari berbagai aspek. Selain ilmu pengetahuan bertambah, dengan membaca buku juga dapat memperkaya pribadi. Maka dari itu, sebisa mungkin membaca buku harus dibiasakan sedari dini agar dapat meningkat minat baca bagi anak serta mengurangi efek negatif seperti kenakalan . Berdasarkan survei yang dilakukan oleh *Program of International Student Assessment* (PISA) tahun 2019 Indonesia berada pada peringkat 62 dari 70 negara berkaitan dengan tingkat literasi. Total jumlah buku bacaan dengan total penduduk Indonesia memiliki rasio 0.09 artinya satu buku ditunggu oleh 90 orang setiap tahunnya [1]. Dengan kemajuan teknologi dan informasi membaca bukan hanya terbatas pada buku saja, terdapat banyak sekali aplikasi baca secara *online*. Di setiap aplikasi tentu saja membutuhkan sistem rekomendasi bagi setiap pembaca. Hal ini dapat mempengaruhi minat baca seseorang dan juga meningkatkan minat pembaca pada aplikasi yang digunakan. 

## Business Understanding
Masalah yang diangkat adalah mengenai buku rekomendasi. Dilihat dari perilaku masyarakat yang sudah terbiasa dengan *gadget* dan dapat menjadi solusi bagi aplikasi-aplikasi baca *online* untuk meningkat minat baca masyarakat Indonesia. 

### Problem Statement
- Bagaimana membuat sistem rekomendasi buku yang dipersonalisasi berdasarkan data pengguna?
- Bagaimana sistem merekomendasikan buku yang mungkin disukai dan belum pernah dibaca oleh pengguna berdasarkan data rating buku?

### Goals
- Menghasilkan sejumlah rekomendasi buku yang dipersonalisasi untuk pengguna.
- Menghasilkan sejumlah rekomendasi buku yang sesuai dengan minat pengguna dan belum pernah dibaca sebelumnya.

### Solution Approach
- **Content Based Filtering**, ide dari algoritma ini adalah merekomendasikan item yang mirip dengan item yang disukai pengguna di masa lampau atau sedang dilihat pada masa sekarang. Content Based Filtering mempelajari profil pengguna berdasarkan data dari objek yang dinilai pengguna. Semakin banyak informasi dari pengguna, maka semakin baik juga performa dari sistem rekomendasi. Semua informasi yang direkomendasikan hanya berdasarkan satu konten saja, sehingga pengguna tidak mendapatkan rekomendasi dari konten yang berbeda. 
- **Collaborative Based Filtering**, Algoritma ini bergantung pada pendapat komunitas pengguna atau penilaian yang diberikan oleh pengguna sebelumnya. Pendekatan ini merekomendasikan item-item yang dilihat atau dipilih oleh pengguna lain dengan kemiripan item dari pengguna saat ini. Jika terdapat item baru dan belum mendapatkan penilai dari pengguna lain makan item tersebut tidak akan direkomendasikan kepada pengguna. 

## Data Understanding
Data yang digunakan merupakan data rekomendasi buku yang saya dapatkan dari [Kaggle](http://kaggle.com). Dataset terdiri dari tiga data yaitu data buku, pengguna, dan rating. Data buku terdiri dari 271.360 baris dan 8 kolom. Data pengguna terdiri dari 279.000 baris dan 8 kolom. Data rating terdiri dari 279.000 baris dan 3 kolom. 
Adapun variabel-variabel dari ketiga data tersebut adalah sebagai berikut:
1. Books
- ISBN : singkatan dari *International Standard Book Number* merupakan kode unik untuk mengidentifikasi buku.
- Book-Title : merupakan judul dari buku
- Book-author : merupakan penulis dari buku
- Year-of-Publication : merupakan tahun dari publikasi buku
- Publisher : merupakan yang mempublikasikan buku 
- Image-URL-S : merupakan gambar dari buku berukuran kecil, link amazon 
- Image-URL-M : merupakan gambar dari buku berukuran medium, link amazon 
- Image-URL-L : merupakan gambar dari buku berukuran besar, link amazon 
2. Ratings
- User-ID : merupakan id dari pengguna
- Book-ISBN : singkatan dari *International Standard Book Number* merupakan kode unik untuk mengidentifikasi buku.
- Book-Rating : merupakan rating dari buku
3. Users
- User-ID : merupakan id dari pengguna
- Location : merupakan lokasi dari pengguna
- Age : merupakan umur dari pengguna

## Data Preparation 
1. **Content Based Filtering**
- Dalam model ini data yang digunakan adalah data buku dan rating
- Mengganti nama fitur pada data rating, buku, dan user agar lebih mudah digunakan dalam memanggil nama fitur. Dalam pemberian nama saya mengganti tanda "-" menjad tanda *underscore* dan juga mengubah huruf menjadi huruf kecil. Misalnya, "User-ID" menjadi "user_id".
- Selanjutnya, menggabungkan data buku dengan data rating yang bertujuan mengetahui rating dari setiap buku yang ada dalam data buku.
- Menghilangkan missing value, dalam data ini terdapat missing value pada fitur book_author, publisher, user_id dan book_rating. Menghilangkan missing value disini karena kita tidak mengindetifikasikan mana buku yang belum memili rating atau belum dinilai oleh pengguna. 
- Menghapus data duplikat agar saat memberikan rekomendasi tidak terdapat buku yang sama.
- Mengurangi data dengan mengambil buku dengan rating diatas 5 dan dibawah 8 agar pengguna mendapatkan rekomendasi buku yang bagus. Karena data yang saya gunakan sangat besar mencapai 1.031.129 saat digabungkan sehingga saat dijalankan terdapat interupsi dan tidak bisa dilanjutkan, hal ini disebabkan karena kapasitas RAM yang tidak mencukupi. 
- Selanjutnya, kita perlu mengkonversi data series menjadi bentuk list. Adapun fitur yang dikonversi adalah ISBN, judul buku, dan penulis buku. Hal ini bertujuan untuk membuat dictionary baru sehingga membentuk sebauh dataframe baru.
- Dilakukan proses vektorisasi dengan TF-IDF. Teknik ini dilakukan dengan tujuan untuk menemukan representasi fitur penting dalam setiap penulis buku
- Lalu mengubah vektor TF-IDF menjadi bentuk matriks dapat menggunakan fungsi todense()
- Menghitung menghitung derajat kesamaan antar buku menggunakan teknik cosine similiarity. 

2. **Collaborative Based Filtering**
- Dalam model data yang digunakan adalah data rating dan data buku.
- Mengurangi data dengan mengambil buku dengan rating diatas 5 dan dibawah 8 pada data rating agar pengguna mendapatkan rekomendasi buku yang bagus
- Dalam persiapan data metode ini, diperlukan proses encoding untuk menyandikan fitur user_id dan ISBN ke dalam indeks integer
- Mengubah tipe data dalam fitur book_rating menjadi float.


## Modeling
1. **Content Based Filtering**
- Pada tahap modeling menggunakan *Content Bases Filtering* dibuat sebuah fungsi yang nanti mengembalikan nilai dalam bentuk dataframe. Parameter yang diperlukan adalah judul buku, similarity_data yang diambil dari nilai *cosine similarity* yang telah dihitung sebelumnya, items yang berisi fitur title dan author, dan k sebanyak 5 yang merupakan jumlah rekomendasi yang diberikan
- Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan lalu mengubah data frame menjadi numpy 
- Mengambil data dengan nilai similarity terbesar dari index yang ada dan menghapus judul buku yang dicari agar tidak muncul dalam daftar rekomendasi
- Dalam sistem rekomendasi buku dengan model ini saya mencari buku dengan judul '*The Firm*' dengan penulis bernama John Grisham. Hasil rekomendasi yang ditampilkan terdapat 5 buku sesuai dengan jumlah k yang diinisialisasi diawal. Kelima hasil rekomendasi dari model ini merupakan buku dengan penulis yang sama yaitu John Grisham.
- Adapun hasil 5 Rekomendasi buku teratas menggunakan model *Content Based Filtering* adalah sebagai berikut:


  <img width="142" alt="top5rekomen" src="https://user-images.githubusercontent.com/50202220/141040609-f78895f6-5973-440f-b442-9aa121f2541e.png">

2. **Collaborative Based Filtering**
- Dalam pemodelan menggunakan model ini perlu menginisialisasi variabel x dan y terlebih dahulu, dimana nilai x merupakan gabungan dari nilai user dan book dari data rating dan nilai y didapatkan dari perhitungan (x - min_rating) / (max_rating - min_rating)
- Membagi data latih dan data validasi dengan presentasi 80% dan 20%
- Membuat class RecommenderNet dengan keras model class untuk membuat skor kecocokan antara user dan buku menggunakan teknik embedding
- Melakukan proses embedding terhadap data user dan data buku
- Melakukan perkalian dot antara embedding user dan embedding buku
- Skor kecocokan diskalakan pada interval [0,1] melalui fungsi aktivasi sigmoid
- Melakukan proses compile terhadap model, dimana loss suction yang digunakan adalah binary cross entropy, fungsi optimasi adalah Adam dengan learning rate 0.001 dan matriks evaluasi yang digunakan adalah Root Mean Squared Error (RMSE)
- Melakukan proses fit terhadap model dimana menggunakan nilai x_train dan y_train sebagai data train dan x_val dan y_val sebagai data validasi, batch size sebesar 8 dan 100 epoch
- Untuk mendapatkan rekomendasi buku, pertama mengambil sampel pengguna secara acak dan didefinisikan pada variabel book_not_read yang merupakan daftar buku yang belum pernah dibaca oleh pengguna. Pengguna telah memberikan penilaian (rating) terhadap buku yang telah dibaca dan rating ini untuk membuat rekomendasi buku sesuai dengan minat pengguna. Buku yang direkomendasi tentu saja buku yang belum pernah dibaca oleh pengguna, maka dari itu diperlukannya membuat variabel book_not_read.
- Lalu, untuk memperoleh rekomendasi buku dapat menggunakan fungsi ```model.predict()``` dari library Keras
- Didapatkan hasil rekomendasi 10 buku dengan rating teratas. Rating tertinggi adalah 8 dan rating terendah adalah 5. 
- Adapun 10 rekomendasi buku teratas menggunakan model *Collaborative Based Filtering* sebagai berikut:


  <img width="433" alt="top10" src="https://user-images.githubusercontent.com/50202220/141040835-352d9092-52b3-4063-9fd0-93925e7d8db0.png">


## Evaluation

1. **Content Based Filtering**
 
  Dalam model ini digunakan nilai presisi sebagai evaluasi. Nilai presisi didapatkan dari jumlah rekomendasi dengan item yang sama dibagikan dengan jumlah seluruh rekomendasi. Presisi sendiri merupakan  pengukuran akurasi dimana label target telah diprediksi
  Dalam program ini saya mencari rekomendasi dari judul buku '*The Firm*' dengan penulis Margaret Atwood. Kelima buku yang direkomendasikan merupakan buku yang ditulis oleh John Grisham sehingga hasil presisi terbilang sempurna. Untuk perhitungan presisi dalam program adalah sebagai berikut:
 ```
a = len(pred[(pred.author == 'John Grisham') + (pred.author == 'JOHN GRISHAM')])
b = len(pred['author'])

precision = a/b
```
Hasil presisi yang didapatkan adalah 1.00 

2. **Collaborative Based Filtering**
  
  Dalam model ini digunakan nilai *Root Mean Squared Error* (RMSE) sebagai evaluasi. RMSE merupakan metode evaluasi dengan mengukur perbedaan nilai dari prediksi sebuah model. Semakin kecil nilai RMSE maka semakin bagus performa dari sebuah model. RMSE ini dinilai efektif diterapkan dalam berbagai bidang, seperti di bidang Ekonomi untuk mengukur apakah model ekonomi dengan indikator ekonomi dan juga di dunia industri dalam melihat nilai akurasi sebuah model, apakah pantas digunakan untuk memprediksi sesuatu di masa mendatang. 
  Hasil *training* yang didapatkan dalam model ini terbilang cukup bagus, dimana nilai RMSE yang didapatkan adalah 0.2175  untuk data latih dan 0.2880 untuk data validasi.
  Dalam program kita dalam mengimplementasi RMSE ini dalam proses *compile* model dengan kode sebagai berikut:
  ```
  model.compile(
    loss = tf.keras.losses.BinaryCrossentropy(),
    optimizer = keras.optimizers.Adam(learning_rate=0.001),
    metrics=[tf.keras.metrics.RootMeanSquaredError()]
 )
 ```
Berikut rumus dari RMSE:

<img width="249" alt="rmse" src="https://user-images.githubusercontent.com/50202220/139662928-2ea705fa-a447-404f-a656-3f9d53666a1d.png">

keterangan:

At : nilai aktual

Ft : nilai prediksi

n : banyaknya data

## Daftar Referensi
[1] Irfan, M., Cahyani, A. D., & R, F. H. (2014). SISTEM REKOMENDASI: BUKU ONLINE DENGAN METODE COLLABORATIVE FILTERING. JURNAL TEKNOLOGI TECHNOSCIENTIA, 7(1), 076–84. https://doi.org/10.34151/technoscientia.v7i1.612

  



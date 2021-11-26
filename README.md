# Laporan Proyek Machine Learning -Bayuzen Ahmad
---
## Domain Proyek
---
Segmentasi merupakan bagian integral dari pengembangan tujuan dan strategi pemasaran, di mana mendefinisikan tujuan tersebut umumnya akan mencakup:
(a) analisis tentang bagaimana produk harus dijual atau dikembangkan, berdasarkan analisis segmen pelanggan saat ini
(b) identifikasi segmen baru sebagai target produk yang sudah ada atau pengembangan produk baru.
Segmentasi sangat penting karena perusahaan memiliki sumber daya yang terbatas, dan harus fokus pada cara terbaik untuk mengidentifikasi dan melayani pelanggannya.
Segmentasi yang efektif memungkinkan perusahaan untuk menentukan kelompok pelanggan mana yang harus mereka layani dan bagaimana memposisikan produk dan layanan mereka dengan baik untuk setiap kelompok.

* Rujukkan referensi yang digunakan untuk membantu penulis dalam memecahkan permasalahan https://medium.com/analytics-vidhya/credit-card-customers-segmentation-bc3c5c87ddc.
---
# Business Understanding
---
Pengembangan segmentasi pelanggan untuk menentukan strategi pemasaran. Sampel Dataset merangkum perilaku penggunaan sekitar 9000 pemegang kartu kredit aktif selama 6 bulan terakhir. File berada pada level pelanggan dengan 18 variabel perilaku.

## Problem Statements
Pada Project ini akan membagi pelanggan menjadi beberapa kelompok untuk menentukan perlakuan yang tepat bagi kelompok pelanggan tersebut sehingga dapat meningkatkan rasa kepuasan pelanggan dan kepercayaan pelanggan terhadap perusahaan.

## Goals
1.Membuat model estimasi segmentasi pelanggan kartu kredit untuk membantu perusahaan dalam menentukan strategi pemasaran.
2.Sampel Dataset merangkum perilaku penggunaan sekitar 9000 pemegang kartu kredit aktif selama 6 bulan terakhir.
3.File berada pada level pelanggan dengan 18 variabel perilaku.
4.Menggunakan algoritma K-Means dengan nilai K ditentukan oleh nilai siluet.
5.Menggunakan PCA untuk pengurangan dimensi dan visualisasi yang lebih baik.

### Solution
* Segmentasi pelanggan kartu kredit menggunakan algoritma K-Means.
K-means adalah algoritma pembelajaran unsupervised (clustering). K-means bekerja dengan mengelompokkan beberapa titik data menjadi satu (clustering) dengan cara yang tidak diawasi. Algoritma mengelompokkan pengamatan dengan nilai atribut yang serupa bersama-sama dengan mengukur jarak Euclidian antar titik.
![image](https://user-images.githubusercontent.com/88529383/143204184-0687373a-e73c-445d-b08a-647ed8ac119d.png)
**Tahapan K-Means Clustering**: <br>
1.Memilih jumlah cluster awal (K) yang ingin dibuat. <br>
2.Memilih titik secara random sebanyak K buah, di mana titik ini akan menjadi pusat (centroid) dari masing-masing kelompok (clusters).<br>
3.Dari dataset yang kita miliki, buat dataset yang terdekat dengan titik centroid sebagai bagian dari cluster tersebut. Sehingga secara total akan terbentuk clusters sebanyak K  buah.<br>
4.Lakukan kalkulasi, dan tempatkan pusat centroid yang baru untuk setiap cluster-nya. Langkah ini bisa disebut juga dengan istilah penyempurnaan centroid.<br>
5.Dari dataset yang kita miliki ambil titik centroid terdekat, sehingga dataset tadi menjadi bagian dari cluster tersebut. Jika masih ada data yang berubah kelompok (pindah cluster), kembali ke langkah 4. Jika tidak, maka cluster yang terbentuk sudah baik.<br>

# Data Understanding
---
Masalah yang dijelaskan dalam kumpulan data ini mengharuskan dilakukan ekstrak segmen pelanggan tergantung pada pola perilaku mereka yang disediakan dalam kumpulan data, untuk memfokuskan strategi pemasaran perusahaan pada segmen tertentu.
sumber data set :https://www.kaggle.com/arjunbhasin2013/ccdata
### Variabel-variabel pada dataset adalah sebagai berikut :
* **CUST_ID** : Identification of Credit Card holder (Categorical). <br>
* **BALANCE** : Balance amount left in their account to make purchases. <br>
* **BALANCE_FREQUENCY** : How frequently the Balance is updated, score between 0 and 1 (1 = frequently updated, 0 = not frequently updated). <br>
* **PURCHASES** : Amount of purchases made from account. <br>
* **ONEOFF_PURCHASES** : Maximum purchase amount done in one-go. <br>
* **INSTALLMENTS_PURCHASES** : Amount of purchase done in installment. <br>
* **CASH_ADVANCE** : Cash in advance given by the user. <br>
* **PURCHASES_FREQUENCY** : How frequently the Purchases are being made, score between 0 and 1 (1 = frequently purchased, 0 = not frequently purchased). <br>
* **ONEOFF_PURCHASES_FREQUENCY** : How frequently Purchases are happening in one-go (1 = frequently purchased, 0 = not frequently purchased). <br>
* **PURCHASES_INSTALLMENTS_FREQUENCY** : How frequently purchases in installments are being done (1 = frequently done, 0 = not frequently done). <br>
* **CASH_ADVANCE_FREQUENCY** : How frequently the cash in advance being paid. <br>
* **CASH_ADVANCE_TRX** : Number of Transactions made with "Cash in Advanced". <br>
* **PURCHASES_TRX** : Numbe of purchase transactions made. <br>
* **CREDIT_LIMIT** : Limit of Credit Card for user. <br>
* **PAYMENTS** : Amount of Payment done by user. <br>
* **MINIMUM_PAYMENTS** : Minimum amount of payments made by user. <br>
* **PRC_FULL_PAYMENT** : Percent of full payment paid by user. <br>
* **TENURE** : Tenure of credit card service for user. <br>

**Data Overview**
![image](https://user-images.githubusercontent.com/88529383/143534632-6cc6e749-db7c-4bd6-a2e1-3defa3dd8e61.png)
Data terdiri dari 8950 baris dan 18 kolom. Berikut ringkasan datanya.

![image](https://user-images.githubusercontent.com/88529383/143534718-380cea1a-7f23-4be2-9c98-8719a36c8875.png)
Ada banyak outlier (lihat nilai maksimalnya), tapi saya tidak menghapusnya karena mungkin berisi informasi penting, jadi saya memperlakukan outlier sebagai nilai ekstrim.

memeriksa type data pada dataset.
![image](https://user-images.githubusercontent.com/88529383/143535069-e2118caf-b123-4393-a43f-cc4c8474f97f.png)
dari informasi terlihat hanya kolom Cust_ID yang object dan jika diperika pada kolom tersebut berisikan nilai unik yang banyak sehingga bisa dipastikan bahwa kolom tersebut tidak akan memberikan informasi, sehingga dalam proses selanjutnya kolom Cust_ID akan didrop.

memriksa missing value pada data.
![image](https://user-images.githubusercontent.com/88529383/143535203-17985c33-5a13-48e6-b3c0-7869e521ccba.png)
kolom pembayaran minimal memiliki missing value sebesar 3.5% dan kolom batas kredit memiliki missing value sebesar 0.01%, karena missing value tergolong sangat kecil maka akan dilakukan penghapusan missing value.

## Eksploration Data Analysis
Eksploration data analysis dilakukan untuk lebih memahami isi data, seperti informasi type tiap kolom, persebaran data dan ringkasan statistik.

* Melakukan pemeriksaan informasi type data pada tiap variable untuk memastikan apakah type data telah sesuai dengan menggunakan fungsi ```info()```.
* Melakukan pendekatan Statistika Descriptive untuk mengenali data berdasarkan statistik menggunakan fungsi ```describe()```
* Memeriksa data yang terduplikasi menggunakan fungsi ```duplicate()``` untuk menghindari terjadinya kesalahan pengumpulan informasi untuk di analisa lebih lanjut.
* memeriksa missing value yang hilang pada tiap variable agar dataset dapat digunakan untuk pembuatan machine learning.
* Melakukan Eksploration data analisis untuk menganalisa data, beberapa hasilnya sebagai berikut :
1.Analisa customer yang loyal
![image](https://user-images.githubusercontent.com/88529383/142851277-792c9059-a826-490f-ab2c-76ac9639d89c.png)

Dari visualisasi, customer didominasi oleh tenure 12 bulan sehingga mengindikasikan bahwa customer puas dengan pelayanan dan customer loyal.
2.Anlisa Customer Menggunakan Bar  dan box plot untuk melihat pola hubungan tiap variabel dengan variabel Tenor
![image](https://user-images.githubusercontent.com/88529383/142851500-d3cdc238-bd4d-494c-88f3-e4063810d675.png)

![image](https://user-images.githubusercontent.com/88529383/142851557-92aa67f5-94d0-43da-afae-99d48b4e8037.png)

3.Matriks Korelasi
matriks korelasi berguna untuk melihat fitur-fitur yang memiliki korelasi satu sama lain sehingga mempermudah untuk memilih fitur-fitur penting yang akan digunakan untuk membangun model. Untuk membuat matriks korelasi dapat menggunakan heat map atau peta panas ```sns.heatmap()``` yang merupakan fungsi dari seaborn.

![image](https://user-images.githubusercontent.com/88529383/143533752-8d877323-d225-406c-b92f-31f7cba9d40a.png)
Dari grafik heatmap terlihat ada banya fitur yang saling berkolerasi kuat, cukup sulit untuk membuat model dengan melakukan pemeilihan fitur secara manual, sehingga proses dalam pembuat model, pereduksian fitur akan dilakukan dengan bantuan PCA
# Data Preparation
---
1. Handle colom categoric
menghandel colom categoric dilakukan dengan cara mengubah type categoric menjadi numerik, hal ini dilakukan karena algoritma machine learning hanya mengenali data berupa numerik dan juga untuk melakukan dimensional reduction atau pengurangan dimensi dataset dengan cara mengurangi fitur-fitur pada dataset tetapi tidak kehilangan banyak informasi apabila terdapat banyak kolom pada dataset dan fitur fiturnya memiliki korelasi satu sama lain, beberapa cara untuk menghadle data dengan tipe categoric adalah dengan menggunakan OneHotEncoder dan LabelEncoder.

2. PRINCIPAL COMPONENT ANALYSIS (PCA)
Dari dataset memiliki banyak fitur, beberapa fitur saling berkolerasi dan beberapa dan tidak semua fitur akan berkontribusi untuk membuat model cluster yang baik, sehingga dilakukan pengurangan dimensi pada dataset dengan menggunakan PRINCIPAL COMPONENT ANALYSIS (PCA).
![image](https://user-images.githubusercontent.com/88529383/143218218-e21e7644-a539-4ac9-adf7-824cd675cfe1.png)

Principal Component Analysis, atau PCA, adalah metode pengurangan dimensi yang sering digunakan untuk mengurangi dimensi kumpulan data besar, dengan mengubah kumpulan besar variabel menjadi lebih kecil yang masih berisi sebagian besar informasi dalam kumpulan besar. Mengurangi jumlah variabel dari kumpulan data secara alami mengorbankan akurasi, tetapi trik dalam pengurangan dimensi adalah menukar sedikit akurasi untuk kesederhanaan. Karena kumpulan data yang lebih kecil lebih mudah untuk dijelajahi dan divisualisasikan serta membuat analisis data menjadi lebih mudah dan lebih cepat untuk algoritme pembelajaran mesin tanpa variabel asing untuk diproses.


# Model
---
* membuat model dari hasil pca untuk menentukan cluster dengan menggunakan algortima KMeans cluster.
* Menggunakan Elbow method untuk melihat cluster terbaik
![image](https://user-images.githubusercontent.com/88529383/142851614-84532357-fe30-4141-bbfa-1048e9cbf78f.png)
* dari grafk menunjukkan K cluster K=3 atau K=4.

# Evaluation
---
Menggunakan silhouette score untuk melihat score pada tiap cluster dan melakukan perbandingan dalam menentukan cluster yang masuk akal, sehingga dapat dilakukan analisa tahap lanjut untuk melihat pola pola pada tiap cluster.
```
range_n_clusters = range(2,10)
for n_clusters in range_n_clusters:
    clusterer = KMeans(n_clusters)
    preds = clusterer.fit_predict(cluster_pca)
    centers = clusterer.cluster_centers_

    score = silhouette_score(cluster_pca, preds)
    print("For n_clusters = {}, silhouette score is {}".format(n_clusters, score))
```
For n_clusters = 2, silhouette score is 0.6138508428489835
For n_clusters = 3, silhouette score is 0.581155923676399
For n_clusters = 4, silhouette score is 0.562598906112909
For n_clusters = 5, silhouette score is 0.4890686346705536
For n_clusters = 6, silhouette score is 0.4869587047832269
For n_clusters = 7, silhouette score is 0.4483910943173569
For n_clusters = 8, silhouette score is 0.4374152749679159
For n_clusters = 9, silhouette score is 0.3971416478419992

diatas adalah hasil dari evaluasi berdasarkan silhoutte score, dari hari yang ditunjukkan cluster terbaik pada jumlah cluster 2, tetapi tidak masuk akal jika customer segmentation sebanyak 2 cluster, sehingga penulis menetapkan jumlah cluster sebanyak 3 karena dianggap cukup masuk akal.

# EDA Tiap Cluster
---
![image](https://user-images.githubusercontent.com/88529383/142851661-404d3e1c-9bb4-45fe-89fa-0e63d5d3af76.png)
dari grafik boxplot dan statistik desricptive rata rata ```clusters.groupby('cluster').mean()``` didapatkan kesimpulan:
Cluster 0
Saldo : Medium 
Frekuensi Saldo : high
Pembelian : High
Frekuensi Pembelian : Medium
Pembayaran Minimal : Medium
Credit Limit : medium

Cluster 1
Saldo : low
Frekuensi Saldo : high
Pembelian : low
Frekuensi Pembelian : low
Pembayaran Minimal : low
Credit Limit : low

Cluster 2
Saldo : High 
Frekuensi Saldo : high
Pembelian : High
Frekuensi Pembelian : High
Pembayaran Minimal : High
Credit Limit : High


## KESIMPULAN
---
untuk cluster 1, rekomendasikan **silver credit card** karena ini adalah kartu yang paling banyak dimiliki. Secara umum, pemegang kartu kredit baru akan menerima kartu perak dan mereka dapat meningkatkannya nanti. Kartu silver memiliki limit kredit paling rendah yaitu sekitar Rp 4 juta hingga Rp 7 juta. Pemegang kartu harus memiliki gaji bulanan minimal Rp 3 juta. Kelebihan dari kartu ini adalah limit yang tidak terlalu tinggi. <br>

untuk cluster 0, rekomendasikan **gold credit card**. Pemegang kartu harus memiliki penghasilan rutin bulanan sekitar Rp 5 juta hingga Rp 10 juta. Limit kredit berkisar antara Rp 10 juta hingga Rp 40 juta, tergantung bank penerbit kartu kredit. Kelebihan dari jenis kartu ini adalah limit yang cukup besar. Jadi, ini memungkinkan Anda untuk membeli/memiliki barang-barang mahal lebih cepat. Anda bisa menggunakannya untuk mencicil barang-barang berbujet besar seperti sepeda motor atau smartphone. Namun, semakin tinggi batas kartu kredit, semakin tinggi biaya tahunan yang harus Anda bayar.

untuk cluster 2, kartu **platinum credit card** dengan level tertinggi. Kartu kredit Platinum hanya dimiliki oleh segelintir orang karena tidak mudah untuk mendapatkan kartu karena prosedur yang ketat. Kartu kredit platinum memiliki limit tinggi mulai dari Rp 40 juta hingga Rp 1 miliar. Pemegang kartu harus memiliki penghasilan minimal Rp 180 juta per tahun dan memiliki riwayat kredit yang baik.



rujukan referensi : https://medium.com/analytics-vidhya/credit-card-customers-segmentation-bc3c5c87ddc

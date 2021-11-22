# Laporan Proyek Machine Learning -Bayuzen Ahmad
---
## Domain Proyek
---
Menganalisa perilaku customer pengguna kartu credit dengan menggunakan KMeans clustering untuk mengclusterisasi customer sehingga didapatkan kelompok untuk menentukan treatmen yang cocok pada tiap kelompok cluster.
* customer memiliki perilaku yang berpola sehingga dapat dikelompokan berdasarkan customer yang memiliki pola perilaku yang sama.
* Rujukkan referensi yang digunakan untuk membantu penulis dalam memecahkan permasalahan https://medium.com/analytics-vidhya/credit-card-customers-segmentation-bc3c5c87ddc.
---
# Business Understanding
---
Customer Card Segmentasi adalah proses mengelompokkan pelanggan pengguna jasa kartu credit berdasarkan karakteristik umum. Kelompok pelanggan ini bermanfaat dalam penentuan treatment, dalam mengidentifikasi pelanggan yang berpotensi menguntungkan, dan dalam mengembangkan loyalitas pelanggan.
## Problem Statements
Pada Project ini kami akan membedakan pelanggan menjadi beberapa kelompok untuk menentukan perlakuan yang tepat bagi kelompok pelanggan tersebut sehingga dapat meningkatkan rasa kepuasan pelanggan dan kepercayaan pelanggan terhadap perusahaan.

## Goals
Membentuk 3 cluster customer pengguna kartu kredit.
### Solution
Menggunakan algoritma KMeans clustering dan mengevaluasi model menggunakan elbow method dan shiloutte, dan mendapatkan cluster terbaik yaitu 3 cluster berbeda.

# Data Understanding
---
Masalah yang dijelaskan dalam kumpulan data ini mengharuskan dilakukan ekstrak segmen pelanggan tergantung pada pola perilaku mereka yang disediakan dalam kumpulan data, untuk memfokuskan strategi pemasaran perusahaan pada segmen tertentu.
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

# Data Preparation
---
* Melakukan pemeriksaan informasi type data pada tiap variable untuk memastikan apakah type data telah sesuai dengan menggunakan fungsi ```info()```.
* Melakukan pendekatan Statistika Descriptive untuk mengenali data berdasarkan statistik menggunakan fungsi ```describe()```
* Memeriksa data yang terduplikasi menggunakan fungsi ```duplicate()``` untuk menghindari terjadinya kesalahan pengumpulan informasi untuk di analisa lebih lanjut.
* memeriksa missing value yang hilang pada tiap variable agar dataset dapat digunakan untuk pembuatan machine learning.
* Melakukan Eksploration data analisis untuk menganalisa data, beberapa hasilnya sebagai berikut :
1. analisa customer yang loyal
![image](https://user-images.githubusercontent.com/88529383/142851277-792c9059-a826-490f-ab2c-76ac9639d89c.png)

Dari visualisasi, customer didominasi oleh tenure 12 tahun sehingga mengindikasikan bahwa customer puas dengan pelayanan dan customer loyal.
2. Anlisa Customer Menggunakan Bar  dan box plot untuk melihat pola hubungan tiap variabel dengan variabel Tenor
![image](https://user-images.githubusercontent.com/88529383/142851500-d3cdc238-bd4d-494c-88f3-e4063810d675.png)

![image](https://user-images.githubusercontent.com/88529383/142851557-92aa67f5-94d0-43da-afae-99d48b4e8037.png)

```PCA
# Data Scaling
scaler = StandardScaler()
#melakukan proses scalling untuk menghilangkan gap antara kolom, dan karena ada ketimpangan pada kolom Tenor maka proses reduksi pca tidak akan memasukkan kolom Tenor
cluster_new[['Saldo','Pembelian', 'Pembelian_Satu_Kali',
       'Pembelian_Angsuran', 'Uang_Dibayar_Dimuka','Transaksi_Uang_Dibayar_Dimuka',
       'Transaksi_Pembelian', 'Batas_Kredit', 'Pembayaran',
       'Pembayaran_Minimal']] = scaler.fit_transform(cluster_new[['Saldo','Pembelian', 'Pembelian_Satu_Kali',
       'Pembelian_Angsuran', 'Uang_Dibayar_Dimuka','Transaksi_Uang_Dibayar_Dimuka',
       'Transaksi_Pembelian', 'Batas_Kredit', 'Pembayaran',
       'Pembayaran_Minimal']])
       
#membunag kolom tenor
cluster_new.drop('Tenor',axis=1,inplace=True)

#menentukan pca comonent
pca = PCA(n_components=2)
pca.fit(cluster_new)
pca_transform = pca.transform(cluster_new)
pca_transform

#menggabungkan hasil PCA
cluster_pca = pd.DataFrame(data=pca_transform,columns=['pca1','pca2'])

# check the explained variance
print('explained variance:',pca.explained_variance_ratio_)
```

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

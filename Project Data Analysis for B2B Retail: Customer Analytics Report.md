# DQLAB-Project Data Analysis for B2B Retail: Customer Analytics Report
![100%](https://user-images.githubusercontent.com/33899480/231225818-f7042c19-b57d-4284-8481-1a38424fedf9.png)

## Latar Belakang
<p align="justify">
xyz.com adalah perusahan rintisan B2B yang menjual berbagai produk tidak langsung kepada end user tetapi ke bisnis/perusahaan lainnya. Sebagai data-driven company, maka setiap pengambilan keputusan di xyz.com selalu berdasarkan data. Setiap quarter xyz.com akan mengadakan townhall dimana seluruh atau perwakilan divisi akan berkumpul untuk me-review performance perusahaan selama quarter terakhir.
<br><b> Hal yang perlu direview:</b>
</p>
![review](https://user-images.githubusercontent.com/33899480/231229962-e5e54b41-c082-42f7-aa6a-a689539845cb.png)

## Tabel Yang Digunakan
<br>
![tabel](https://user-images.githubusercontent.com/33899480/231231784-35924c91-0437-4771-858e-a5dbd88c0d8a.png)

<br>

## Memahami Tabel
<p align="justify">
Sebelum memulai menyusun query SQL dan membuat Analisa dari hasil query, hal pertama yang perlu dilakukan adalah menjadi familiar dengan tabel yang akan digunakan. Hal ini akan sangat berguna dalam menentukan kolom mana sekiranya berkaitan dengan problem yang akan dianalisa, dan proses manipulasi data apa yang sekiranya perlu dilakukan untuk kolom – kolom tersebut, karena tidak semua kolom pada tabel perlu untuk digunakan. 
<br>Lakukan pengecekan data menggunakan query di bawah ini:</p>
<details>
  <summary>Click to show query sql</summary>

```
SELECT * FROM orders_1 limit 5;
SELECT * FROM orders_2 limit 5;
SELECT * FROM customer limit 5;
```
</details>
<p>
 Hasil dari query di atas seperti ini :
 </p>
  ![all table](https://user-images.githubusercontent.com/33899480/231238488-836b90b9-bb7c-411e-a395-de128b45f572.png)
  
# Bagaimana Pertumbuhan Penjualan Saat Ini



  ## Menghitung Total Penjualan dan Revenue pada Quarter-1 (Jan, Feb, Mar) dan Quarter-2 (Apr,Mei,Jun)
  <p>
 Untuk mendapatkan total penjualan dan revenue pada quarter 1 dan quarter 2 maka perlu melakukan urutan langkah berikut:
   </p>
 <ol>
   <li> Dari tabel orders_1 lakukan penjumlahan pada kolom quantity dengan fungsi aggregate sum() dan beri nama “total_penjualan”, kalikan kolom quantity dengan kolom priceEach kemudian jumlahkan hasil perkalian kedua kolom tersebut dan beri nama “revenue”</li>
<li>Perusahaan hanya ingin menghitung penjualan dari produk yang terkirim saja, jadi kita perlu mem-filter kolom ‘status’ sehingga hanya menampilkan order dengan status “Shipped”.</li>
   <li>Lakukan Langkah 1 & 2, untuk tabel orders_2.</li>
   </ol>
 
 <details>
  <summary>Click to show query sql</summary>

```
select sum(quantity) as total_penjualan, sum(quantity*priceEach) as revenue from orders_1 where status="Shipped" ;
select sum(quantity) as total_penjualan, sum(quantity*priceEach) as revenue from orders_2 where status="Shipped";
```
</details>
 <p>
  Hasil Query diatas seperti dibawah ini :
  </p>
  ![totalrevenue](https://user-images.githubusercontent.com/33899480/231241132-cd607cdc-2579-459f-95a3-0bff054c217d.png)

## Menghitung persentasi keseluruhan penjualan

<p align="justify">
  Untuk menghitung  persentasi keseluruhan penjualan maka perlu menggabungkan tabel orders_1 dan orders_2.
  <br>
  Langkah-langkah untuk mendapatkan presentasi keseluruhan penjualan sebagai berikut: 
  </p>
  <ol>
  <li>Pilihlah kolom “orderNumber”, “status”, “quantity”, “priceEach” pada tabel orders_1, dan tambahkan kolom baru dengan nama “quarter” dan isi dengan value “1”. Lakukan yang sama dengan tabel orders_2, dan isi dengan value “2”, kemudian gabungkan kedua tabel tersebut.</li>
  <li>Gunakan statement dari Langkah 1 sebagai subquery dan beri alias “tabel_a”.</li>
<li>Dari “tabel_a”, lakukan penjumlahan pada kolom “quantity” dengan fungsi aggregate sum() dan beri nama “total_penjualan”, dan kalikan kolom quantity dengan kolom priceEach kemudian jumlahkan hasil perkalian kedua kolom tersebut dan beri nama “revenue”</li>
  <li>Filter kolom ‘status’ sehingga hanya menampilkan order dengan status “Shipped”.</li>
<li>Kelompokkan total_penjualan berdasarkan kolom “quarter”, dan jangan lupa menambahkan kolom ini pada bagian select.</li>
  </ol>
  
  <details>
  <summary>Click to show query sql</summary>

```
SELECT quarter, sum(quantity) AS total_penjualan, sum(quantity*priceEach) AS revenue FROM (SELECT orderNumber,status,quantity,priceEach,'1' quarter FROM orders_1 WHERE status='Shipped'UNION
SELECT orderNumber,status,quantity,priceEach,'2' quarter FROM orders_2 WHERE status='Shipped')as table_a GROUP BY quarter;
```
</details>
 <p>
  Hasil Query diatas seperti dibawah ini :
  </p>
  ![presentasi](https://user-images.githubusercontent.com/33899480/231244731-c4baabcc-4f86-4912-bc52-9470376849ce.png)

## Perhitungan Growth Penjualan dan Revenue
![growth](https://user-images.githubusercontent.com/33899480/231245446-fbfbeae9-fd57-4b21-950b-b682b366286b.png)

# Customer Analytics

## Apakah jumlah customers xyz.com semakin bertambah?
<p>
  Penambahan jumlah customers dapat diukur dengan membandingkan total jumlah customers yang registrasi di quarter 2 dengan  total jumlah customers yang registrasi quarter 1.
  <br> Langkah - Langkah mencari total customer setiap quarter
   </p>
<ol>
  <li>
Dari tabel customer, pilihlah kolom customerID, createDate dan tambahkan kolom baru dengan menggunakan fungsi QUARTER(…) untuk mengekstrak nilai quarter dari CreateDate dan beri nama “quarter”</li>
<li>Filter kolom “createDate” sehingga hanya menampilkan baris dengan createDate antara 1 Januari 2004 dan 30Juni 2004</li>
  <li>Gunakan statement Langkah 1 & 2 sebagai subquery dengan alias tabel_b</li>
<li>Hitunglah jumlah unik customers à tidak ada duplikasi customers dan beri nama “total_customers”</li>
<li>Kelompokkan total_customer berdasarkan kolom “quarter”, dan jangan lupa menambahkan kolom ini pada bagian select.</li>
  
 <details>
  <summary>Click to show query sql</summary>

```
SELECT quarter, COUNT(DISTINCT(customerID)) AS total_customers FROM (
  SELECT customerID,createDate,QUARTER(createDate) AS quarter FROM customer WHERE  createDate BETWEEN '2004-01-01' AND '2004-06-30') AS tabel_b GROUP BY quarter;
```
</details>
 <p>
  Hasil Query diatas seperti dibawah ini :
  </p>
  ![totalcs](https://user-images.githubusercontent.com/33899480/231246757-e4aacfbe-6db2-4cbc-87bf-c0c9565d81bc.png)

 <p>
   Hasil query menunjukan bahwa jumlah customer tidak semakin bertambah, tetapi jumlah customer mengalami penurunan dari yang sebelumnya 43 customer di quarter 1 menurun menjadi 35 customer di quarter 2. </p>

 ## Seberapa banyak customers tersebut yang sudah melakukan transaksi?
  
  <p align="justify">
 Problem ini merupakan kelanjutan dari problem sebelumnya yaitu dari sejumlah customer yang registrasi di periode quarter-1 dan quarter-2, berapa banyak yang sudah melakukan transaksi
  </p>
  <ol type="1">
<li>Dari tabel customer, pilihlah kolom customerID, createDate dan tambahkan kolom baru dengan menggunakan fungsi QUARTER(…) untuk mengekstrak nilai quarter dari CreateDate dan beri nama “quarter”</li>
<li>Filter kolom “createDate” sehingga hanya menampilkan baris dengan createDate antara 1 Januari 2004 dan 30 Juni 2004</li>
    <li>Gunakan statement Langkah A&B sebagai subquery dengan alias tabel_b</li>
<li>Dari tabel orders_1 dan orders_2, pilihlah kolom customerID, gunakan DISTINCT untuk menghilangkan duplikasi, kemudian gabungkan dengan kedua tabel tersebut dengan UNION.</li>
<li>Filter tabel_b dengan operator IN() menggunakan 'Select statement langkah 4' , sehingga hanya customerID yang pernah bertransaksi (customerID tercatat di tabel orders) yang diperhitungkan.</li>
<li>Hitunglah jumlah unik customers (tidak ada duplikasi customers) di statement SELECT dan beri nama “total_customers”</li>
<li>Kelompokkan total_customer berdasarkan kolom “quarter”, dan jangan lupa menambahkan kolom ini pada bagian select.</li>
  </ol>
     <details>
  <summary>Click to show query sql</summary>

```
SELECT quarter, COUNT(DISTINCT(customerID)) AS total_customers 
FROM (
	  SELECT customerID,createDate,QUARTER(createDate) AS quarter FROM 
	  customer WHERE  createDate BETWEEN '2004-01-01' AND '2004-06-30'
		) AS tabel_b 
WHERE customerID IN (
	  SELECT DISTINCT(customerID) FROM orders_1 
	  UNION 
	  SELECT DISTINCT(customerID) FROM orders_2)
GROUP BY quarter;
```
</details>
 <p>
  Hasil Query diatas seperti dibawah ini :
  </p>
  ![totalcs](https://user-images.githubusercontent.com/33899480/231249573-16e31f09-680a-4f09-bdd1-6be79e5af11b.png)

   <p>
   Hasil query menunjukan bahwa jumlah customer yang melakukan transaksi pada quarter 1 sebanyak 25 customerdan 19 customer di quarter 2. </p>

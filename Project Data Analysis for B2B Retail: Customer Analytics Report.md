# DQLAB-Project Data Analysis for B2B Retail: Customer Analytics Report
![100%](https://user-images.githubusercontent.com/33899480/231225818-f7042c19-b57d-4284-8481-1a38424fedf9.png)

## Latar Belakang
<p align="justify">
xyz.com adalah perusahan rintisan B2B yang menjual berbagai produk tidak langsung kepada end user tetapi ke bisnis/perusahaan lainnya. Sebagai data-driven company, maka setiap pengambilan keputusan di xyz.com selalu berdasarkan data. Setiap quarter xyz.com akan mengadakan townhall dimana seluruh atau perwakilan divisi akan berkumpul untuk me-review performance perusahaan selama quarter terakhir.
</p>
<b> Hal yang perlu direview:</b>

![review](https://user-images.githubusercontent.com/33899480/231229962-e5e54b41-c082-42f7-aa6a-a689539845cb.png)

## Tabel Yang Digunakan
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
</p>
<b>
 Hasil dari query di atas seperti ini :
 </b>
 
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
</p>
 <b>
  Hasil Query diatas seperti dibawah ini :
  </b>
  
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
</p>
 <b>
  Hasil Query diatas seperti dibawah ini :
  </b>
  
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
	</p>
 <b>
  Hasil Query diatas seperti dibawah ini :
  </b>
  
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
</p>
 <b>
  Hasil Query diatas seperti dibawah ini :
  </b>
  
  ![totalcs](https://user-images.githubusercontent.com/33899480/231249573-16e31f09-680a-4f09-bdd1-6be79e5af11b.png)

   <p>
   Hasil query menunjukan bahwa jumlah customer yang melakukan transaksi pada quarter 1 sebanyak 25 customerdan 19 customer di quarter 2. </p>

## Category produk apa saja yang paling banyak di-order oleh customers di Quarter-2?
<p align="justify">
	Untuk mendapatkan category produk yang paling banyak diorder oleh customer pada quarter ke-2, maka diperlukan perhitungan untuk total order dan jumlah penjualan dari setiap produk.
	<br>
	Langkah-langkah yang bisa dilakukan sebagai berikut : 
</p>
<ol>
<li>Dari kolom orders_2, pilih productCode, orderNumber, quantity, status</li>
<li>Tambahkan kolom baru dengan mengekstrak 3 karakter awal dari productCode yang merupakan ID untuk kategori produk; dan beri nama categoryID</li>
<li>Filter kolom “status” sehingga hanya produk dengan status “Shipped” yang diperhitungkan</li>
<li>Gunakan statement Langkah 1, 2, dan 3 sebagai subquery dengan alias tabel_c</li>
<li>Hitunglah total order dari kolom “orderNumber” dan beri nama “total_order”, dan jumlah penjualan dari kolom “quantity” dan beri nama “total_penjualan”</li>
<li>Kelompokkan berdasarkan categoryID, dan jangan lupa menambahkan kolom ini pada bagian select.</li>
<li>Urutkan berdasarkan “total_order” dari terbesar ke terkecil</li>
	</ol>
	
 <details>
  <summary>Click to show query sql</summary>

```
select SUBSTRING(productcode,1,3) as categoryid,count(distinct ordernumber) as total_order,sum(quantity) as total_penjualan from 
(select productcode,ordernumber,quantity,status,SUBSTRING(productcode,1,3) as categoryid from orders_2 where status = "Shipped") tabel_c
group by SUBSTRING(productcode,1,3)
order by count(distinct ordernumber)desc;
```
</details></p>
 <b>
  Hasil Query diatas seperti dibawah ini :
  </b>

![order categori](https://user-images.githubusercontent.com/33899480/231573655-0acbaf86-f464-4f70-a6e3-c1d765ac1a7a.png)

   <p>
   Hasil query menunjukan bahwa ada 8 kategori yang paling banyak diorder dengan categoryid S18 merupakan category dengan orderan tertinggi.</p>
	
## Seberapa banyak customers yang tetap aktif bertransaksi setelah transaksi pertamanya?
<p align="justify">
	Customer yang tetap aktif bertransaksi usai transaksi pertamanya merupakan indikator bahwa xyz.com tetap digemari customer untuk memenuhi kebutuhan bisnis mereka. Customer yang tetap aktif bertransaksi juga dapat digunakan untuk pengambilan keputusan dasar bagi tim product dan bisnis untuk strategi pengembangn product dan bisnis kedepannya. Adapun metrik yang digunakan disebut retention cohort. Untuk project ini, kita akan menghitung retention dengan query SQL sederhana, sedangkan cara lain yaitu JOIN dan SELF JOIN.
</p>
<p align="justify">	
Dalam menghitung retention, maka perhitungannya yaitu dengan meghitung retention customer pada quarter 1 dan tetap melakukan transaksi di quarter 2.	
<br>
Untuk langkah dari perhitungan sebagai berikut:
</p>
<ol type="1">
<li>Dari tabel orders_1, tambahkan kolom baru dengan value “1” dan beri nama “quarter”</li>
<li>Dari tabel orders_2, pilihlah kolom customerID, gunakan distinct untuk menghilangkan duplikasi</li>
<li>Filter tabel orders_1 dengan operator IN() menggunakan 'Select statement langkah 2', sehingga hanya customerID yang pernah bertransaksi di quarter 2 (customerID tercatat di tabel orders_2) yang diperhitungkan.</li>
<li>Hitunglah jumlah unik customers (tidak ada duplikasi customers) dibagi dengan total_ customers dalam percentage, pada Select statement dan beri nama “Q2”</li>

 <details>
  <summary>Click to show query sql</summary>

```
#Menghitung total unik customers yang transaksi di quarter_1
SELECT COUNT(DISTINCT customerID) as total_customers FROM orders_1;
#output = 25
SELECT '1' quarter,((COUNT(DISTINCT customerID))*100)/25 AS Q2 FROM orders_1 WHERE customerID IN(
  SELECT DISTINCT(customerID) FROM orders_2);
```
</details>
	</p>
	<b>  Hasil Query diatas seperti dibawah ini :</b>

![customer aktif](https://user-images.githubusercontent.com/33899480/231578879-a399f0f0-fbfd-4ce0-8395-ee9d83bb53f7.png)

   <p>
   Hasil query menunjukan retention rate customer xyz.com sebesar 24%</p>
   
   
   # Kesimpulan Analisa
   Berdasarkan data yang diperoleh dari query SQL yang telah dilakukan, maka kesimpulan yang dapat ditarik adalah:
<ol type="1">
<li>Performance xyz.com menurun signifikan di quarter ke-2, terlihat dari nilai penjualan dan revenue yang drop hingga 20% dan 24%,</li>
<li>perolehan customer baru juga tidak terlalu baik, dan sedikit menurun dibandingkan quarter sebelumnya.</li>
<li>Ketertarikan customer baru untuk berbelanja di xyz.com masih kurang, hanya sekitar 56% saja yang sudah bertransaksi. Disarankan tim Produk untuk perlu mempelajari behaviour customer dan melakukan product improvement, sehingga conversion rate (register to transaction) dapat meningkat.</li>
<li>Produk kategori S18 dan S24 berkontribusi sekitar 50% dari total order dan 60% dari total penjualan, sehingga xyz.com sebaiknya fokus untuk pengembangan category S18 dan S24.</li>
<li>Retention rate customer xyz.com juga sangat rendah yaitu hanya 24%, artinya banyak customer yang sudah bertransaksi di quarter-1 tidak kembali melakukan order di quarter ke-2 (no repeat order).</li>
<li>xyz.com mengalami pertumbuhan negatif di quarter ke-2 dan perlu melakukan banyak improvement baik itu di sisi produk dan bisnis marketing, jika ingin mencapai target dan positif growth di quarter ke-3. Rendahnya retention rate dan conversion rate bisa menjadi diagnosa awal bahwa customer tidak tertarik/kurang puas/kecewa berbelanja di xyz.com.</li></ol>

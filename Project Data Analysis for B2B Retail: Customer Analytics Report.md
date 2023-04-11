# DQLAB-Project Data Analysis for B2B Retail: Customer Analytics Report
![100%](https://user-images.githubusercontent.com/33899480/231225818-f7042c19-b57d-4284-8481-1a38424fedf9.png)

## Latar Belakang
<p align="justify">
xyz.com adalah perusahan rintisan B2B yang menjual berbagai produk tidak langsung kepada end user tetapi ke bisnis/perusahaan lainnya. Sebagai data-driven company, maka setiap pengambilan keputusan di xyz.com selalu berdasarkan data. Setiap quarter xyz.com akan mengadakan townhall dimana seluruh atau perwakilan divisi akan berkumpul untuk me-review performance perusahaan selama quarter terakhir.
<br><b> Hal yang perlu direview:</b>
</p>
![review](https://user-images.githubusercontent.com/33899480/231229962-e5e54b41-c082-42f7-aa6a-a689539845cb.png)

<br>
## Tabel Yang Digunakan<br>
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

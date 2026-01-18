# Project UAS Basis Data – Cafe Nescafe

## 📌 Deskripsi Project Basis Data
Project ini merupakan tugas Ujian Akhir Semester (UAS) mata kuliah Basis Data yang bertujuan untuk merancang dan mengimplementasikan sistem basis data pada sebuah cafe bernama **Nescafe**. Sistem ini dibuat menggunakan aplikasi **MySQL Workbench** untuk mengelola data operasional cafe, seperti data shift kerja, frontliner, barang, pelanggan, dan transaksi penjualan.  
Database dirancang dengan relasi antar tabel menggunakan **Primary Key** dan **Foreign Key** untuk menjaga integritas data serta mendukung proses pengolahan dan pelaporan transaksi.

---

## 🛠️ Tools yang Digunakan
Berikut tools yang digunakan dalam pengerjaan proyek ini:
- **XAMPP Control Panel** - sebagai lingkungan server lokal untuk menjalankan database
- **MySQL Workbench / phpMyAdmin** – untuk mengelola dan menjalankan query database
- **GitHub** – untuk menyimpan dan mendokumentasikan proyek
- **Microsoft Word** - untuk membuat laporan project
- **PDF Reader** – untuk membuka laporan tugas dalam bentuk PDF

---

## ⚙️ Langkah Setup Database
Ikuti langkah-langkah berikut untuk menjalankan database:

1. **Buka MySQL**
   -  Connect kan XAMPP Control Panel
   -  Jalankan MySQL Server melalui MySQL Workbench atau phpMyAdmin.

2. **Buat Database**
   Jalankan perintah berikut:
   ```sql
   CREATE DATABASE nescafe;
USE nescafe;

CREATE TABLE shift (
id_shift int,
nama_shift char(20),
jam_mulai time,
jam_ishoma time,
jam_selesai time,
primary key (id_shift)
);
describe shift;

CREATE TABLE frontliner (
id_frontliner int,
nama_frontliner char(50),
posisi char(20),
id_shift int,
primary key (id_frontliner),
foreign key (id_shift) references shift(id_shift)
);
describe frontliner;

CREATE TABLE barang (
id_barang int,
nama_barang char(50),
harga int,
primary key (id_barang)
);
describe barang;

CREATE TABLE transaksional (
order_id char(20),
tanggal date,
jenis_order char(10),
nama_barang char(50),
harga int,
total int,
cash int,
kembalian int,
order_nomor int,
id_frontliner int,
primary key (order_id),
foreign key (id_frontliner) references frontliner(id_frontliner),
foreign key (harga) references barang(harga)
);
describe transaksional;

CREATE TABLE pelanggan (
order_nomor int,
nama_pelanggan char(50),
tanggal date,
nama_barang char(50),
order_id char(20),
id_barang int,
primary key (nama_pelanggan),
foreign key (order_id) references transaksional(order_id)
);
describe pelanggan;

insert into pelanggan values
('0104', 'kuat', '2026-10-19', 'dalgona coffe', 'NE/20251016/104','3747'),
('0105', 'naya', '2026-10-19', 'coffe latte','NE/20251016/105','3748'),
('0106', 'rafi', '2026-10-19', 'americano','NE/20251016/106','3749');

select *from pelanggan;

insert into shift values
(0815, 'pagi', '8:00', '12:00-13:00', '15:00'),
(1522, 'siang', '15:00', '15:00-22:00', '22:00');

select *from shift;

insert into frontliner values
(104620, 'yessi sabitha', 'kasir', 0815),
(104621, 'ahmad fajar khoirul', 'barista', 0815),
(104622, 'dimas wibi anggoro', 'cleaning', 0815),
(104623, 'raden fajar risky', 'kasir', 1522),
(104624, 'farhanazahra rahma', 'barista', 1522);

select *from frontliner;

insert into barang values
(3747, 'dalgona coffe', 10.000),
(3748, 'coffe latte', 8.000),
(3749, 'americano', 5.000);

select *from barang;

insert into transaksional values
('NE/20251016/104', '2026-10-19', 'take away', 10.000, 10.000, 10.000, 0, 0104, 104620),
('NE/20251016/105', '2026-10-19', 'take away', 8.000, 8.000, 8.000, 0, 0105, 104621),
('NE/20251016/106', '2026-10-19', 'take away', 5.000, 5.000, 5.000, 0, 0106, 104621);

select *from transaksional;

INSERT INTO transaksional (order_id, tanggal, jenis_order, harga, total, cash, kembalian, order_nomor, id_frontliner)
VALUES ('NE/20251016/106', '2026-10-19', 'take away', 5.000, 5.000, 5.000, 0, 0106, 104621);

ALTER TABLE pelanggan
ADD CONSTRAINT fk_pelanggan_barang
FOREIGN KEY (id_barang)
REFERENCES barang(id_barang);

ALTER TABLE pelanggan
ADD CONSTRAINT fk_pelanggan_order
FOREIGN KEY (order_id)
REFERENCES transaksional(order_id);

ALTER TABLE pelanggan
ADD id_barang int;

ALTER TABLE pelanggan
ADD order_id char;

ALTER TABLE barang
MODIFY nama_barang char(50);

ALTER TABLE pelanggan
MODIFY order_id char(20);

ALTER TABLE pelanggan
MODIFY order_nomor int;

ALTER TABLE transaksional
MODIFY tanggal date;

alter table pelanggan
change tanggal_order tanggal date;

alter table transaksional
change subtotal harga int;

select nama_pelanggan,order_nomor
from pelanggan;
select nama_barang,harga
from barang
order by harga DESC;

select * from barang;
select distinct id_barang
from barang;
select distinct id_barang,nama_barang
from barang;


select * from barang
where id_barang and harga > '6.000';
select * from barang
where id_barang;

select * from barang
WHERE harga>=6.000 and harga<=9.000
order by harga;

select * from barang
where harga between 6.000 and 10.000;

select nama_barang, harga, 0.15*harga as 'total'
from barang;
select nama_barang, harga as buy_price, 0.15*harga as diskon
from barang;
select nama_barang, harga as buy_price,
15/100 * harga as diskon, harga - diskon
from barang;
select nama_barang, id_barang, harga,
15/100 * harga as diskon,
harga - 15/100, harga as 'total'
from barang;

select min(harga) as harga_terkecil,
max(harga) as harga_terbesar,
avg(harga) as rata_rata,
sum(harga) as total_harga,
count(nama_barang) as banyak_jenis
from barang;

select id_frontliner,nama_frontliner,count(*) as total
from frontliner
where id_shift=0815
group by id_frontliner
order by total;

select nama_frontliner,posisi,count(id_frontliner) as jumlah_frontliner
from frontliner
where id_frontliner = '104620'
group by nama_frontliner
having count(id_frontliner)
order by nama_frontliner;

select shift.jam_mulai,jam_selesai, frontliner.nama_frontliner
from shift
inner join frontliner
on shift.id_shift = frontliner.id_shift;

select frontliner.nama_frontliner, transaksional.order_id
from frontliner
left join transaksional
on frontliner.id_frontliner = transaksional.id_frontliner;

select frontliner.nama_frontliner, shift.jam_mulai
from frontliner
right join shift
on frontliner.id_shift = shift.id_shift;

SELECT frontliner.nama_frontliner, shift.nama_shift
FROM frontliner
LEFT JOIN shift
ON frontliner.id_shift = shift.id_shift

UNION

SELECT frontliner.nama_frontliner, shift.nama_shift
FROM frontliner
RIGHT JOIN shift
ON frontliner.id_shift = shift.id_shift;

START TRANSACTION;
INSERT INTO transaksional
(order_id, tanggal, jenis_order, harga, total, cash, kembalian, order_nomor, id_frontliner)
VALUES
('NE/20251016/106', '2026-10-19', 'take away', 8000, 8000, 10000, 2000, 0106, 104623);
COMMIT;

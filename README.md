# Sales Insights Cafe and Bakery-
This repository contains datasets, scripts, and documentation for storing, managing, and analyzing company sales data.

Project ini bertujuan untuk membangun dan menganalisis database penjualan Café & Bakery menggunakan PostgreSQL, dengan analisis data yang dapat dihubungkan ke Tableau Public untuk visualisasi.

### Project Overview

Repository ini berisi kumpulan datasets, SQL scripts, dan dokumentasi analisis data penjualan perusahaan, yang dirancang untuk mempelajari proses:
1. Membuat & mengelola database PostgreSQL
2. Mengimpor data penjualan
3. Menjalankan analisis menggunakan SQL
4. Menghubungkan database ke Tableau Public untuk visualisasi

### Database Schema Overview

Database ini berisi 4 tabel utama:
- products: Menyimpan daftar produk dan harga
- customers: Informasi pelanggan
- markets: Lokasi & channel penjualan
- transactions: Catatan setiap transaksi penjualan

  ### Folder Explanation

- | `datasets/`                     | Berisi file `.csv` | Data mentah produk, pelanggan, transaksi       |
- | `scripts/`                      | Berisi file `.sql` | Script untuk membuat dan menganalisis database |
- | `README.md`                     | Dokumentasi utama  | Penjelasan project dan cara menjalankan        |
- | `tableau_dashboard_preview.png` | (Opsional)         | Screenshot visualisasi di Tableau              |

### How to Use the Project
#### Create Database
 ```bash
CREATE DATABASE company_sales;
```

#### Run Table Creation Script
Jalankan file scripts/create_tables.sql
 ```bash
CREATE TABLE products (
  product_code VARCHAR(10) PRIMARY KEY,
  product_name VARCHAR(50),
  category VARCHAR(30),
  cost_price NUMERIC(10,2),
  sell_price NUMERIC(10,2)
);

CREATE TABLE customers (
  customer_code VARCHAR(10) PRIMARY KEY,
  customer_name VARCHAR(50),
  gender VARCHAR(10),
  city VARCHAR(30),
  join_date DATE
);

CREATE TABLE markets (
  market_code VARCHAR(10) PRIMARY KEY,
  market_name VARCHAR(30),
  channel_type VARCHAR(20)
);

CREATE TABLE transactions (
  transaction_id SERIAL PRIMARY KEY,
  order_date DATE,
  product_code VARCHAR(10) REFERENCES products(product_code),
  customer_code VARCHAR(10) REFERENCES customers(customer_code),
  market_code VARCHAR(10) REFERENCES markets(market_code),
  qty INT,
  total_sales NUMERIC(10,2),
  profit NUMERIC(10,2),
  profit_margin NUMERIC(5,2)
);
 ```

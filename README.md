# 📊  Sales Insights Cafe and Bakery-

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

#### Insert Sample Data

 ```bash
INSERT INTO products VALUES
('P001','Croissant','Bakery',7000,15000),
('P002','Chocolate Cake','Dessert',12000,25000),
('P003','Iced Coffee','Beverage',8000,18000),
('P004','Latte','Beverage',10000,20000),
('P005','Donut','Snack',6000,12000);

INSERT INTO customers VALUES
('C001','Andi','Male','Medan','2023-01-10'),
('C002','Budi','Male','Siantar','2023-02-12'),
('C003','Sinta','Female','Medan','2023-03-05'),
('C004','Rina','Female','Parapat','2023-04-20'),
('C005','Tono','Male','Siantar','2023-05-01');

INSERT INTO markets VALUES
('M001','Café Downtown','Offline'),
('M002','Café Online','Online'),
('M003','Café Express','Offline');

INSERT INTO transactions(order_date, product_code, customer_code, market_code, qty, total_sales, profit, profit_margin) VALUES
('2024-01-10','P001','C001','M001',3,45000,24000,53.3),
('2024-01-15','P002','C002','M002',2,50000,26000,52.0),
('2024-02-01','P003','C003','M003',4,72000,36000,50.0),
('2024-03-12','P004','C004','M001',1,20000,10000,50.0),
('2024-03-30','P005','C005','M002',6,72000,36000,50.0);
 ```
#### Run Analysis Queries

Gunakan scripts/analysis_queries.sql, berisi contoh analisis:

a. Total Sales per Product
 ```bash
SELECT p.product_name, SUM(t.qty) AS total_qty, SUM(t.total_sales) AS total_sales
FROM transactions t
JOIN products p ON p.product_code = t.product_code
GROUP BY p.product_name
ORDER BY total_sales DESC;
```

b. Monthly Profit Trends per Product
 ```bash
SELECT TO_CHAR(order_date, 'YYYY-MM') AS bulan, SUM(profit) AS total_profit
FROM transactions
GROUP BY TO_CHAR(order_date, 'YYYY-MM')
ORDER BY bulan;
```
c. Average Margin per Category
```bash
SELECT p.category, ROUND(AVG(t.profit_margin),2) AS avg_margin
FROM transactions t
JOIN products p ON p.product_code = t.product_code
GROUP BY p.category
ORDER BY avg_margin DESC;
```

d. Top 5 Customers
```bash
SELECT c.customer_name, SUM(t.total_sales) AS total_spent
FROM transactions t
JOIN customers c ON c.customer_code = t.customer_code
GROUP BY c.customer_name
ORDER BY total_spent DESC
LIMIT 5;
```
Credit: 
Proyek ini dibuat sebagai simulasi analisis data penjualan pada bisnis Café & Bakery. Data dan struktur database dikembangkan dengan bantuan Artificial Intelligence (AI) melalui ChatGPT (by OpenAI) untuk tujuan pembelajaran dan portofolio.


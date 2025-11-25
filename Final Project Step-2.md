### Creating a SQLite Database and Tables

Start a command prompt In the project’s database directory  and run sqlite3 data.db to initialize a new SQLite database named data.db.

```
sqlite3 data.db;
```

**Create products table**

```
CREATE TABLE products (
    barcode TEXT PRIMARY KEY,
    name TEXT NOT NULL,
    threshold INTEGER NOT NULL
);
```

**Insert data into products table**

```
INSERT INTO products (name, barcode, threshold) VALUES
('Gillette Mach3 Razor Blades (8 ct)', '047400245909', 80),
('Kellogg’s Corn Flakes 18 oz', '038000001019', 90),
('Coca-Cola Classic 12 oz Can', '049000028911', 120),
('Tylenol Extra Strength 100 ct', '300450444305', 40),
('Colgate Total Toothpaste 4.8 oz', '035000521019', 50);
```

**Create the inventory table**

```
CREATE TABLE inventory (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    barcode TEXT NOT NULL,
    quantity INTEGER NOT NULL,
    transaction_date TEXT DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (barcode) REFERENCES products(barcode)
);
```

**Insert data into inventory table.**

```
INSERT INTO inventory (name, barcode, quantity, transaction_date) VALUES
('Gillette Mach3 Razor Blades (8 ct)', '047400245909', 90, '2025-10-01 07:45:00'),
('Kellogg’s Corn Flakes 18 oz', '038000001019', 120, '2025-10-01 08:30:00'),
('Coca-Cola Classic 12 oz Can', '049000028911', 100, '2025-10-01 09:00:00'),
('Tylenol Extra Strength 100 ct', '300450444305', 70, '2025-10-01 10:00:00'),
('Colgate Total Toothpaste 4.8 oz', '035000521019', 60, '2025-10-01 11:00:00'),

('Kellogg’s Corn Flakes 18 oz', '038000001019', -25, '2025-10-02 12:00:00'),
('Colgate Total Toothpaste 4.8 oz', '035000521019', -10, '2025-10-02 13:00:00'),
('Coca-Cola Classic 12 oz Can', '049000028911', -20, '2025-10-02 14:30:00'),
('Tylenol Extra Strength 100 ct', '300450444305', -15, '2025-10-02 15:00:00'),
('Gillette Mach3 Razor Blades (8 ct)', '047400245909', -10, '2025-10-02 16:00:00'),

('Kellogg’s Corn Flakes 18 oz', '038000001019', -35, '2025-10-03 14:00:00'),
('Colgate Total Toothpaste 4.8 oz', '035000521019', -15, '2025-10-03 15:00:00'),
('Coca-Cola Classic 12 oz Can', '049000028911', -30, '2025-10-03 16:45:00'),
('Tylenol Extra Strength 100 ct', '300450444305', -20, '2025-10-03 17:00:00'),
('Gillette Mach3 Razor Blades (8 ct)', '047400245909', -15, '2025-10-03 18:00:00'),

('Kellogg’s Corn Flakes 18 oz', '038000001019', 60, '2025-10-04 09:00:00'),
('Coca-Cola Classic 12 oz Can', '049000028911', 50, '2025-10-04 10:15:00'),
('Tylenol Extra Strength 100 ct', '300450444305', 40, '2025-10-04 11:00:00'),
('Gillette Mach3 Razor Blades (8 ct)', '047400245909', 30, '2025-10-04 12:00:00'),
('Colgate Total Toothpaste 4.8 oz', '035000521019', 30, '2025-10-04 17:00:00');
```

**Create logs table**

```
CREATE TABLE logs (
    id INTEGER PRIMARY KEY,
    timestamp TEXT,
    action TEXT,
    details TEXT
);
```
**Exit Sqlite**

```
.exit
```
### Creating a SQLite Database and Tables

---

The recommended way is to **re-create the project** based on the instructions in the following section.  
However, if you prefer to clone the project directly, use the following Git command to review and modify it in your local Python environment:

```bash
git clone -b step-2 https://github.com/solomontessema/smart-inventory-agent
cd smart-inventory-agent
```

Or, if you want to open it in Google Colab, click the badge below:

<a href="https://colab.research.google.com/github/solomontessema/Agentic-AI-with-Python/blob/main/notebooks/Final%20Project/inventory_agent_step_2.ipynb" target="_parent"> <img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/> </a>

---

Start a command prompt In the project’s database directory  and run sqlite3 data.db to initialize a new SQLite database named data.db.

```
sqlite3 data.db;
```

**Create products table**

```
CREATE TABLE products (
    barcode TEXT PRIMARY KEY,
    name TEXT NOT NULL
);
```

**Insert data into products table**

```
INSERT INTO products (name, barcode) VALUES
('Gillette Mach3 Razor Blades (8 ct)', '047400245909'),
('Kellogg’s Corn Flakes 18 oz', '038000001019'),
('Coca-Cola Classic 12 oz Can', '049000028911'),
('Tylenol Extra Strength 100 ct', '300450444305'),
('Colgate Total Toothpaste 4.8 oz', '035000521019');
```

**Create transactions table**

```
CREATE TABLE Transactions (
  transaction_id INTEGER PRIMARY KEY,   -- auto-increments automatically in SQLite
  barcode TEXT NOT NULL,
  transaction_type TEXT CHECK(transaction_type IN ('PURCHASE','SALE','RETURN','DAMAGED')),
  quantity INTEGER NOT NULL,
  transaction_date DATETIME DEFAULT CURRENT_TIMESTAMP,
  FOREIGN KEY (barcode) REFERENCES Products(barcode)
);
```

**Insert data into transactions table.**

```
-- Gillette Mach3 Razor Blades (8 ct)
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('047400245909','PURCHASE',40);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('047400245909','SALE',5);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('047400245909','SALE',3);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('047400245909','RETURN',2);

-- Kellogg’s Corn Flakes 18 oz
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('038000001019','PURCHASE',60);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('038000001019','SALE',10);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('038000001019','SALE',8);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('038000001019','DAMAGED',3);

-- Coca-Cola Classic 12 oz Can
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('049000028911','PURCHASE',100);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('049000028911','SALE',12);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('049000028911','SALE',15);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('049000028911','RETURN',5);

-- Tylenol Extra Strength 100 ct
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('300450444305','PURCHASE',50);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('300450444305','SALE',6);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('300450444305','DAMAGED',2);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('300450444305','SALE',4);

-- Colgate Total Toothpaste 4.8 oz
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('035000521019','PURCHASE',70);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('035000521019','SALE',9);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('035000521019','RETURN',1);
INSERT INTO Transactions (barcode, transaction_type, quantity) VALUES ('035000521019','DAMAGED',2);

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
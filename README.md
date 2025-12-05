# prestashop-docker-deployment
Containerized PrestaShop environment deployed using Docker and Docker Compose for E-Commerce Systems Engineering course project.



# PrestaShop Docker Deployment  
A Complete Containerized E-Commerce Environment

## 📌 Project Overview

This repository contains the practical implementation for **Assignment 0** of the course *E-Commerce Systems Engineering & Management*.  
The objective of this project is to:

- Install and configure **Docker**
- Deploy the open-source e-commerce platform **PrestaShop** using Docker Compose
- Explore PrestaShop’s frontend (customer side) and back office (admin side)
- Evaluate its **multi-store** capability
- Document the deployment with videos and a technical report

The result is a fully operational, containerized e-commerce environment running on a local machine.

---

## 🐳 Technologies Used

| Component     | Version / Type |
|---------------|----------------|
| Docker        | Latest |
| Docker Compose| v3.3 |
| PrestaShop    | Latest image |
| MySQL         | 5.7 |
| OS            | Windows / Linux compatible |

---

## 🧱 Architecture Summary

The system consists of two main containers:

1. **MySQL (Database Server)**  
2. **PrestaShop (Application Server)**  

Both are connected through Docker networking, and persistent storage is handled using volumes:

```

mysql_data        → Stores MySQL database files
prestashop_data   → Stores PrestaShop application files

````

---

## ⚙️ Docker Compose Configuration

The exact configuration used in this project:

```yaml
version: '3.3'

services:

  mysql:
    image: mysql:5.7
    container_name: prestashop-mysql
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: prestashop
      MYSQL_USER: prestauser
      MYSQL_PASSWORD: prestapass
    volumes:
      - mysql_data:/var/lib/mysql

  prestashop:
    image: prestashop/prestashop:latest
    container_name: prestashop-app
    restart: always
    depends_on:
      - mysql
    ports:
      - "8080:80"
    environment:
      DB_SERVER: mysql
      DB_NAME: prestashop
      DB_USER: prestauser
      DB_PASSWD: prestapass
      PS_DEV_MODE: "1"
      PS_INSTALL_AUTO: "0"
      PS_DOMAIN: "localhost:8080"
    volumes:
      - prestashop_data:/var/www/html

volumes:
  mysql_data:
  prestashop_data:
````

---

## ▶️ How to Run the Project

### **1️⃣ Start the containers**

```bash
docker compose up -d
```

### **2️⃣ Access the store (frontend)**

👉 [http://localhost:8080](http://localhost:8080)

### **3️⃣ Complete the installation in browser**

Use these database settings:

| Field    | Value      |
| -------- | ---------- |
| Host     | mysql      |
| Database | prestashop |
| Username | prestauser |
| Password | prestapass |

### **4️⃣ Access the Admin Panel**

PrestaShop generates a unique admin URL such as:

```
http://localhost:8080/admin123xyz
```

💡 Save this link — PrestaShop displays it only once.

---

## 🛍 Features Demonstrated

### **Frontend (Customer Area)**

* Browse categories
* View product details
* Add items to cart
* Complete checkout steps
* Register or log in as a user

### **Back Office (Admin Area)**

* Dashboard overview
* Product management
* Category management
* Order processing
* Customer management
* Store configuration
* Module installation
* Advanced shop settings

---

## 🏪 Multi-Store Support in PrestaShop

PrestaShop provides powerful multi-store capabilities:

✔ Multiple independent shops
✔ Separate domains or subdomains
✔ Shared or exclusive products
✔ Centralized or independent configurations

To enable:

**Back Office → Shop Parameters → General → Enable Multistore**

Use cases include:

* Chain stores
* Franchises
* Multi-brand platforms
* Wholesale/Retail hybrid stores
* Supply chain–based commerce systems

---

## 🛠 Troubleshooting Guide

### ❗ MySQL connection fails

Check if containers are running:

```bash
docker ps
```

View logs:

```bash
docker logs prestashop-mysql
docker logs prestashop-app
```

---

### ❗ PrestaShop in installation loop

Run:

```bash
docker compose down
docker compose up -d
```

---

### ❗ Forgot admin URL

Inside the PrestaShop container:

```bash
docker exec -it prestashop-app bash
ls /var/www/html/
```

Look for folder starting with `admin...`.

---

## 📁 Recommended Repository Structure

```
prestashop-docker-deployment/
│
├── docker-compose.yml
├── README.md
├── .gitignore
│
├── docs/
│   └── report.pdf
│
├── screenshots/
│   ├── frontend.png
│   ├── admin-panel.png
│   └── docker.png
│
└── videos/
    ├── demo-short-link.txt
    └── demo-full-link.txt
```

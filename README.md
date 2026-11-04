# FS-01: Multi-Vendor E-Commerce Marketplace - Technical Documentation

An Amazon-style marketplace where many vendors sell, customers buy, and the platform splits payments and earns commission.

## 1. Database Schema & Tables

### A. Users Table
- user_id (Primary Key / UUID)
- name (String)
- email (String, Unique)
- password (Hashed String)
- role (Enum: CUSTOMER, VENDOR, ADMIN)
- created_at (Timestamp)

### B. Products Table
- product_id (Primary Key / UUID)
- vendor_id (Foreign Key -> References Users)
- title (String)
- description (Text)
- price (Decimal)
- stock_quantity (Integer)
- image_url (String - AWS S3 Link)
- created_at (Timestamp)

### C. Orders Table
- order_id (Primary Key / UUID)
- customer_id (Foreign Key -> References Users)
- total_amount (Decimal)
- payment_status (Enum: PENDING, COMPLETED, FAILED)
- order_status (Enum: PLACED, PACKED, SHIPPED, DELIVERED)
- created_at (Timestamp)

### D. Order_Items Table
- order_item_id (Primary Key / UUID)
- order_id (Foreign Key -> References Orders)
- product_id (Foreign Key -> References Products)
- quantity (Integer)
- price (Decimal)

---

## 2. REST API Endpoints

### Authentication APIs:
- POST /api/auth/register - User registration
- POST /api/auth/login - User login (Returns JWT Token)

### Product APIs:
- GET /api/products - Get all products with search & filters
- POST /api/products - Create new product (Vendor only)
- PUT /api/products/:id - Update product details
- DELETE /api/products/:id - Delete product

### Order & Checkout APIs:
- POST /api/orders - Place order and initiate payment
- GET /api/orders/customer/:id - Get orders by customer ID
- PUT /api/orders/:id/status - Update order status (PLACED -> PACKED -> SHIPPED -> DELIVERED)

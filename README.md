# Ecommerce Web Application

This ecommerce platform was developed as part of the **Web Development Fundamentals** course at **Qatar University**. It demonstrates a full-stack web application featuring multiple user roles and core ecommerce functionalities using modern technologies like Prisma ORM and React.

---

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [User Roles](#user-roles)
- [Technology Stack](#technology-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Environment Setup](#environment-setup)
  - [Database Setup](#database-setup)
- [Running the Application](#running-the-application)
  - [Part 1: Basic HTML/JS Version](#part-1-basic-htmljs-version)
  - [Admin Dashboard (React)](#admin-dashboard-react)
- [Database Schema](#database-schema)
- [Use Cases](#use-cases)
- [Sample Data](#sample-data)

---

## Project Overview

This ecommerce application supports three distinct user types:

- **Customer**: Browses and purchases items.
- **Seller**: Uploads and manages items for sale, views sales history.
- **Admin**: Oversees platform management through an admin dashboard built with React.

The app is split into two parts:

- **Part 1**: Basic implementation without React or Next.js. Consists of static HTML, CSS, and JavaScript files served via Live Server.
- **Admin Dashboard**: Built with React to provide a modern interface for admins.

The backend uses Prisma ORM connected to a SQLite database. Users and items are managed through JSON files for simplicity.

---

## Features

- User authentication via predefined usernames and passwords stored in JSON files.
- Item searching and filtering on the main page.
- Purchase flow with quantity selection and shipping address input.
- Automatic updates to bank and money balances upon successful purchase.
- Sellers can upload new items, update quantities, and view sales histories.
- Admin dashboard for managing users and overseeing platform activities.
- Purchase and sales histories maintained and viewable by respective users.

---

## User Roles

| Role     | Capabilities                                                   |
| -------- | --------------------------------------------------------------|
| Customer | - Search and view items<br>- Purchase items if logged in<br>- View purchase history |
| Seller   | - Upload new items<br>- Update existing items<br>- View sales and sale history |
| Admin    | - Manage platform through React dashboard<br>- View/manage users and sales |

---

## Technology Stack

- **Frontend**: HTML, CSS, JavaScript, React (Admin Dashboard)
- **Backend**: Node.js, Prisma ORM
- **Database**: SQLite
- **Tools**: Live Server (for Part 1), Prisma CLI

---

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/en/download/) (v14+ recommended)
- npm (comes with Node.js)
- [Live Server extension](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) for VSCode (for Part 1)
- Optional: SQLite installed (not mandatory, Prisma can handle the SQLite file creation)

---

### Installation

1. Clone the repository:

   ```bash
   git clone <repo-url>
   cd <repo-folder>
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

### Environment Setup

Create a `.env.local` file in the root directory with the following content:

```env
DATABASE_URL="file:./dev.db"
```

This points Prisma to use a local SQLite database file named `dev.db`.

### Database Setup

1. Run Prisma migrations to initialize the database schema:

   ```bash
   npx prisma migrate dev --name init
   ```

2. Generate the Prisma client:

   ```bash
   npx prisma generate
   ```

---

## Running the Application

### Part 1: Basic HTML/JS Version

1. Open your code editor.
2. Run Live Server on the project folder.
3. Open this URL in your browser:

   ```
   http://localhost:<port>/public/Part-1/index.html
   ```

- This version does not use React or Next.js.
- Users are authenticated via static JSON files.

### Admin Dashboard (React)

To run the React-based admin dashboard and backend API:

```bash
npm run dev
```

This will start the Next.js development server (or React if standalone) typically at:

```
http://localhost:3000
```

Open this URL to access the admin dashboard interface.

---

## Database Schema

The database schema is managed by Prisma with the following models:

### Seller

| Field | Type | Description |
|-------|------|-------------|
| sellerId | Int | Primary key, auto-increment |
| username | String | Seller login username |
| password | String | Seller password |
| company_name | String | Seller's company name |
| bank_account_balance | Int | Seller's bank account balance |
| Item | Relation | Items sold by this seller |

### Item

| Field | Type | Description |
|-------|------|-------------|
| id | Int | Primary key, auto-increment |
| name | String | Item name |
| type | String | Item category/type |
| price | Float | Price of the item |
| image | String | Image URL or path |
| material | String | Material of the item |
| gender | String | Target gender (if applicable) |
| color | String | Color of the item |
| description | String | Description text |
| quantity | Int | Available quantity |
| sellerId | Int | Foreign key to Seller |
| Purchase | Relation | Purchases of this item |

### Purchase

| Field | Type | Description |
|-------|------|-------------|
| purchaseId | Int | Primary key, auto-increment |
| quantity | Int | Number of items purchased |
| purchaseDate | DateTime | Timestamp of purchase |
| itemId | Int | Foreign key to Item |
| customerId | Int | Foreign key to Customer |

### Customer

| Field | Type | Description |
|-------|------|-------------|
| customerId | Int | Primary key, auto-increment |
| username | String | Customer login username |
| password | String | Customer password |
| name | String | Customer's first name |
| surname | String | Customer's last name |
| shipping_address | String | Customer's shipping address |
| money_balance | Int | Available money balance |
| Purchase | Relation | Purchases made by the customer |

---

## Use Cases

### 1. Login
- Users log in with username and password stored in `users.json`.
- No registration; user data is predefined.
- After login, users are redirected to the main page.

### 2. Search Available Items
- Users can search for items using keywords or filters.
- Results display only matching items.

### 3. Purchase Item
- **Precondition**: User must be logged in.
- Customers can purchase one or more units of a single item per transaction.
- On purchase, users input quantity and shipping details.
- System checks if the user has sufficient balance.
- Updates customer money balance and seller's bank account.
- Records purchase history.

### 4. View Purchase History
- Customers can view their previous purchases with details.

### 5. Seller Sales Management
- Sellers can view their active listings.
- View sales history including who bought items and prices.
- Upload new items or update existing item quantities.

### 6. Upload Items for Sale
- Sellers logged in can upload new items with all necessary details.
- If item exists, they can update the quantity.

---

## Sample Data

### Sellers (in `sellers.json`)

```json
[
  {
    "sellerId": 1,
    "username": "Magic Inc.",
    "password": "abracadabra",
    "company_name": "Magic Inc.",
    "bank_account_balance": 0
  },
  {
    "sellerId": 2,
    "username": "Fantasy Traders",
    "password": "mythical321",
    "company_name": "Fantasy Traders",
    "bank_account_balance": 0
  }
]
```

### Admin User (in `admin.json`)

```json
{
  "username": "admin",
  "password": "password"
}
```

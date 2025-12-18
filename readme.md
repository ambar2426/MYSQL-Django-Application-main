## MySQL Django Car Dealership Application

This repository contains a **Django-based car dealership web application** backed by a relational schema originally designed for MySQL. It provides separate flows for admins, dealers, and customers, with car listings, search, and simple market analysis tools built on top of the underlying database.

> GitHub repo: [`MYSQL-Django-Application-main`](https://github.com/ambar2426/MYSQL-Django-Application-main)

---

### Features

- **Customer survey & registration**
  - Collect basic customer details (name, address, phone, gender, income).
  - Persist customers to the `customer` table and redirect them into the search flow.

- **Dealer registration & login**
  - Register new dealers with ID, name, and password.
  - Login dealers and show inventory availability for selected brands/models/styles.

- **Admin raw SQL console**
  - Simple admin login backed by the `admins` table.
  - Run ad‑hoc SQL queries against the car database from the web UI and inspect results.

- **Car search**
  - Browse unsold inventory across dealers by brand, model, and style.
  - See per‑dealer inventory counts and prices for matching vehicles.

- **Market analysis**
  - Basic query builder interface to explore `deal`, `dealer_inventory`, and related tables.
  - Run grouping/aggregation queries without leaving the browser.

- **Predefined schema & data generator**
  - `data/DDL.sql` contains the full SQL DDL for the car schema (vehicle, price, engines, transmissions, options, dealer_inventory, deal, customer, plant_inventory, admins, dealers).
  - `data/DataGeneration.py` and its companion `.txt` files can be used to generate large test datasets and insert statements into `relations.sql`.

---

### Tech stack

- **Backend**: Python 3.x, Django 4.1.x
- **Database**:
  - Default: SQLite (`db.sqlite3`) for quick local runs.
  - Original design: MySQL (schema in `data/DDL.sql`).
- **Frontend**: Django templates, Bootstrap 3, jQuery, custom CSS.
- **Other**: Raw SQL via `django.db.connection` for reporting/analytics views.

---

### Project structure (high level)

- `mysite/` – Django project root
  - `mysite/` – project settings, URLs, WSGI/ASGI
  - `company/` – main app (models, views, templates, URLs)
  - `static/` – CSS, JS, fonts, and car images
- `data/`
  - `DDL.sql` – database schema
  - `DataGeneration.py` – bulk data generation script
  - `relations.sql` – generated insert statements (created by the script)

---

### Getting started (SQLite, easiest)

This project is preconfigured to run with **SQLite** so you can try it without installing MySQL.

1. **Clone the repository**

   ```bash
   git clone https://github.com/ambar2426/MYSQL-Django-Application-main.git
   cd MYSQL-Django-Application-main/mysite
   ```

2. **Create and activate a virtual environment (recommended)**

   ```bash
   python -m venv venv
   venv\Scripts\activate  # Windows
   # source venv/bin/activate  # Linux/macOS
   ```

3. **Install dependencies**

   ```bash
   pip install "Django>=4.1,<4.2"
   ```

4. **Apply migrations**

   ```bash
   python manage.py migrate
   ```

5. **Create a superuser (optional, for Django admin site)**

   ```bash
   python manage.py createsuperuser
   ```

6. **Run the development server**

   ```bash
   python manage.py runserver
   ```

7. **Open the app**

   Visit `http://127.0.0.1:8000/` in your browser.

---

### Running with MySQL (original setup)

If you want to run the project against a real **MySQL** database:

1. **Create a MySQL database**

   ```sql
   CREATE DATABASE car CHARACTER SET utf8mb4;
   ```

2. **Load the schema**

   From the MySQL client, run:

   ```sql
   SOURCE path/to/data/DDL.sql;
   ```

3. **(Optional) Generate and load sample data**

   - Adjust paths in `data/DataGeneration.py` if necessary.
   - Run it to generate `relations.sql`:

     ```bash
     cd data
     python DataGeneration.py
     ```

   - In MySQL:

     ```sql
     SOURCE path/to/data/relations.sql;
     ```

4. **Configure Django to use MySQL**

   In `mysite/mysite/settings.py`, replace the SQLite `DATABASES` section with the original MySQL configuration (or update credentials as needed):

   ```python
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'car',
           'USER': 'root',
           'PASSWORD': 'test',
           'HOST': 'localhost',
           'PORT': '3306',
       }
   }
   ```

5. **Install a MySQL driver**

   ```bash
   pip install mysqlclient
   ```

6. **Run the server**

   ```bash
   cd ../mysite
   python manage.py runserver
   ```

---

### Main URLs / pages

- **`/`** – Home page showing top‑selling cars for a given year.
- **`/adminLogin/`** – Admin login page (checks against the `admins` table).
- **`/admin/`** – Simple SQL console for running custom queries.
- **`/dealerLogin/`** – Dealer login page.
- **`/dealerReg/`** – Dealer registration form.
- **`/dealer/`** – Dealer view with inventory availability for selected cars.
- **`/custReg/`** – Customer registration / survey form.
- **`/search/`** – Customer search for available cars across dealers.
- **`/market/`** – Market analysis page with a flexible query builder.

---

### License

This project is licensed under the **MIT License** – see the `LICENSE` file for details.


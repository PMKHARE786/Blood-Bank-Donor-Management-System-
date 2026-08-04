# 🩸 Blood Bank & Donor Management System (BBDMS)

A full-featured **Blood Bank & Donor Management System** built with **PHP** and **MySQL**, designed to digitize and streamline the process of blood donation, donor registration, blood inventory tracking, and administrative management for blood banks and hospitals.

![PHP](https://img.shields.io/badge/PHP-777BB4?style=for-the-badge&logo=php&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white)
![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-563D7C?style=for-the-badge&logo=bootstrap&logoColor=white)

---

## 📋 Table of Contents

1. [About the Project](#-about-the-project)
2. [Key Features](#-key-features)
3. [Tech Stack](#-tech-stack)
4. [System Architecture](#-system-architecture)
5. [Database Design](#-database-design)
6. [Installation Guide](#-installation-guide)
7. [Folder Structure](#-folder-structure)
8. [User Roles & Modules](#-user-roles--modules)
9. [Screenshots](#-screenshots)
10. [Security Considerations](#-security-considerations)
11. [Future Enhancements](#-future-enhancements)
12. [Known Issues](#-known-issues)
13. [Contributing](#-contributing)
14. [License](#-license)
15. [Contact](#-contact)

---

## 📖 About the Project

The **Blood Bank & Donor Management System (BBDMS)** is a web-based application that connects blood donors, hospitals, and blood bank administrators on a single platform. It solves the real-world problem of **manual, paper-based blood bank record keeping** by digitizing donor registration, blood stock management, donation history, and request processing.

The system is designed for use by:
- **Blood banks / hospitals** that need to manage donor databases and blood inventory
- **Donors** who want to register, view their donation history, and respond to urgent blood requests
- **Administrators** who oversee the entire blood bank operation — approvals, stock levels, and reporting

This project was built as a demonstration of core web development concepts using PHP and MySQL, following the classic **LAMP/WAMP/XAMPP** stack approach, making it easy to run locally for learning, academic submission, or as a base for a production-grade system.

---

## ✨ Key Features

### For Donors
- 🧾 Self-registration with personal, medical, and contact details
- 🔑 Secure login system
- 🩸 View blood group and eligibility status
- 📅 Track donation history and last donation date
- 🔔 Receive/view urgent blood requirement notifications
- ✏️ Update profile information

### For Admin
- 📊 Centralized dashboard with key statistics (total donors, total stock, pending requests)
- 🗂️ Manage donor records (add, edit, delete, search, filter by blood group)
- 🧪 Manage blood stock/inventory by blood group (A+, A-, B+, B-, AB+, AB-, O+, O-)
- 📥 Approve or reject donor registrations
- 📤 Manage blood requests raised by hospitals/patients
- 📈 Generate and view reports (donation trends, stock levels)
- 🔍 Search donors by blood group, location, or availability

### General
- 🔐 Role-based access (Admin / Donor)
- 📱 Responsive UI (works on desktop, tablet, and mobile browsers)
- 🗄️ MySQL-backed persistent storage
- ⚙️ Simple, dependency-light deployment (no frameworks required)

---

## 🛠️ Tech Stack

| Layer              | Technology              |
|---------------------|--------------------------|
| Frontend            | HTML5, CSS3, JavaScript, Bootstrap |
| Backend             | PHP (Core PHP)           |
| Database            | MySQL                    |
| Server Environment  | Apache (via XAMPP / WAMP / LAMP) |
| Database Management | phpMyAdmin               |

---

## 🏗️ System Architecture

The application follows a traditional **3-tier architecture**:

```
┌─────────────────────┐
│   Presentation Tier   │  → HTML, CSS, JS, Bootstrap (Browser)
└──────────┬───────────┘
           │  HTTP Request/Response
┌──────────▼───────────┐
│   Application Tier     │  → PHP scripts (business logic,
│                        │     session handling, validation)
└──────────┬───────────┘
           │  SQL Queries
┌──────────▼───────────┐
│     Data Tier           │  → MySQL Database (bbdms)
└─────────────────────┘
```

- The **browser** sends requests to PHP scripts on form submission (login, register, donate, request blood, etc.)
- **PHP** processes the request, validates input, applies business rules, and interacts with MySQL
- **MySQL** stores and returns donor records, blood stock, requests, and admin data
- PHP renders the response back as dynamic HTML

---

## 🗄️ Database Design

The system uses a MySQL database named **`bbdms`**. Below is the expected high-level schema (import via `bbdms.sql`):

### Core Tables

**1. `users` / `admin`**
| Column        | Type          | Description                  |
|---------------|---------------|-------------------------------|
| id            | INT (PK)      | Unique identifier             |
| username      | VARCHAR(100)  | Login username                |
| password      | VARCHAR(255)  | Hashed password                |
| role          | VARCHAR(20)   | admin / donor                 |
| created_at    | DATETIME      | Account creation timestamp     |

**2. `donors`**
| Column          | Type          | Description                     |
|-----------------|---------------|-----------------------------------|
| donor_id        | INT (PK)      | Unique donor ID                   |
| full_name       | VARCHAR(100)  | Donor's full name                 |
| email           | VARCHAR(100)  | Donor's email (login identifier)  |
| phone           | VARCHAR(15)   | Contact number                    |
| blood_group     | VARCHAR(5)    | A+, A-, B+, B-, AB+, AB-, O+, O-  |
| age             | INT           | Donor age                         |
| gender          | VARCHAR(10)   | Male / Female / Other             |
| address         | TEXT          | Residential address               |
| last_donation   | DATE          | Date of last donation             |
| status          | VARCHAR(20)   | Active / Inactive / Pending       |

**3. `blood_stock`**
| Column        | Type          | Description                    |
|---------------|---------------|----------------------------------|
| stock_id      | INT (PK)      | Unique record ID                 |
| blood_group   | VARCHAR(5)    | Blood group                      |
| units_available | INT        | Number of units in stock         |
| updated_at    | DATETIME      | Last stock update timestamp      |

**4. `blood_requests`**
| Column         | Type          | Description                        |
|----------------|---------------|--------------------------------------|
| request_id     | INT (PK)      | Unique request ID                    |
| requester_name | VARCHAR(100)  | Name of hospital/patient requesting  |
| blood_group    | VARCHAR(5)    | Requested blood group                |
| units_needed   | INT           | Number of units required             |
| contact_number | VARCHAR(15)   | Contact for coordination             |
| status         | VARCHAR(20)   | Pending / Approved / Rejected        |
| request_date   | DATETIME      | Date/time of request                 |

**5. `donation_history`**
| Column         | Type          | Description                    |
|----------------|---------------|----------------------------------|
| donation_id    | INT (PK)      | Unique donation record ID        |
| donor_id       | INT (FK)      | References `donors.donor_id`     |
| donation_date  | DATE          | Date of donation                 |
| units_donated  | INT           | Units donated                    |
| location       | VARCHAR(100)  | Donation center/camp location    |

> 📌 **Note:** The exact schema may vary slightly based on the version of `bbdms.sql` included in the project package. Always refer to the SQL file for the authoritative structure.

---

## ⚙️ Installation Guide

### Prerequisites
- [XAMPP](https://www.apachefriends.org/) / [WAMP](https://www.wampserver.com/) / LAMP stack installed
- A web browser (Chrome, Firefox, Edge)
- Basic familiarity with phpMyAdmin

### Step-by-Step Setup

**1. Download / Clone the project**
```bash
git clone https://github.com/<your-username>/Blood-Bank-Donor-Management-System.git
```
Or download the ZIP and extract it.

**2. Copy project files**
Copy the extracted `bbdms` folder into your server's root directory:
- XAMPP → `C:\xampp\htdocs\bbdms`
- WAMP → `C:\wamp64\www\bbdms`
- LAMP (Linux) → `/var/www/html/bbdms`

**3. Start Apache & MySQL**
Open the XAMPP/WAMP control panel and start both **Apache** and **MySQL** services.

**4. Create the database**
- Open [http://localhost/phpmyadmin](http://localhost/phpmyadmin)
- Click **New** → create a database named exactly `bbdms`

**5. Import the database**
- Select the `bbdms` database
- Go to the **Import** tab
- Choose the `bbdms.sql` file (found inside the `SQL/` folder of the project package)
- Click **Go**

**6. Configure database connection**
Open the database config file (commonly `config.php` or `db_connect.php`) and verify/update the credentials:
```php
$host = "localhost";
$username = "root";
$password = "";
$database = "bbdms";
```

**7. Run the project**
Open your browser and navigate to:
```
http://localhost/bbdms
```

**8. Login**
Use the admin or donor credentials provided with the project package, or register a new donor account from the registration page.

> ⚠️ **Security Note:** Default/demo credentials shipped with academic or sample projects should **never** be used in a production/public deployment. Change all default passwords immediately, and avoid publishing credentials in a public GitHub repository.

---

## 📁 Folder Structure

```
bbdms/
│
├── admin/                  # Admin panel pages (dashboard, manage donors, stock, requests)
│   ├── dashboard.php
│   ├── manage_donors.php
│   ├── manage_stock.php
│   └── manage_requests.php
│
├── donor/                  # Donor-facing pages
│   ├── register.php
│   ├── login.php
│   ├── profile.php
│   └── donation_history.php
│
├── includes/                # Shared PHP includes
│   ├── db_connect.php       # Database connection
│   ├── header.php
│   └── footer.php
│
├── assets/                  # Static assets
│   ├── css/
│   ├── js/
│   └── images/
│
├── SQL/                     # Database schema
│   └── bbdms.sql
│
├── index.php                 # Landing / login page
└── README.md                 # Project documentation
```

> 📌 Actual folder/file names may differ slightly depending on the specific build of the project — refer to the codebase for the exact layout.

---

## 👥 User Roles & Modules

### 1. Admin Module
- Login with admin credentials
- View dashboard with total donors, blood units, and pending requests
- Add/Edit/Delete donor records
- Update blood stock levels per blood group
- Approve/reject incoming blood requests
- View and export reports

### 2. Donor Module
- Register with personal & medical details
- Login to personal dashboard
- View/update profile
- View donation history
- View current blood availability
- Respond to urgent donation calls (if implemented)

### 3. Guest/Public Module (if implemented)
- View blood availability by blood group (read-only)
- Submit a blood request without logging in
- Contact/help information

---

## 🖼️ Screenshots

> Add screenshots of your running application here for a more visual README. Example placeholders:

```
![Login Page](screenshots/login.png)
![Admin Dashboard](screenshots/admin-dashboard.png)
![Donor Registration](screenshots/donor-register.png)
![Blood Stock Management](screenshots/blood-stock.png)
```

---

## 🔒 Security Considerations

This project was originally built as an academic/demo system. Before using it in any real-world or public-facing environment, consider the following improvements:

- **Password Hashing:** Ensure all passwords are stored using `password_hash()` (bcrypt) rather than plain text or weak hashing.
- **SQL Injection Protection:** Use prepared statements (`PDO` or `mysqli` with bound parameters) instead of directly concatenating user input into SQL queries.
- **Session Security:** Implement proper session timeout, regeneration on login, and CSRF protection on forms.
- **Input Validation:** Sanitize and validate all user inputs on both client and server side.
- **HTTPS:** Deploy behind HTTPS in production to protect data in transit.
- **Remove Default Credentials:** Never leave demo/default admin credentials active in a live deployment.
- **File Upload Validation:** If the system supports profile picture or document uploads, strictly validate file types and sizes to prevent malicious uploads.
- **Environment Variables:** Move database credentials out of source code and into environment variables or a `.env` file excluded from version control.

---

## 🚀 Future Enhancements

- 📧 Email/SMS notifications for urgent blood requirements
- 📍 Location-based donor search (nearby donors)
- 📱 Dedicated mobile app (Android/iOS)
- 📊 Advanced analytics dashboard with charts (donation trends, seasonal demand)
- 🔔 Automated reminders for donors eligible to donate again (typically after 90 days)
- 🏥 Hospital/blood bank multi-branch support
- 🌐 Multi-language support
- 🔐 Two-factor authentication for admin accounts
- 📄 PDF export for donor certificates and reports
- 🤝 Integration with government blood bank APIs (e.g., eRaktKosh in India)

---

## 🐞 Known Issues

- Default demo credentials are shipped with the project — must be changed before any public/production use
- Some forms may lack complete server-side validation
- No automated testing suite included
- UI may require further responsive design polishing on smaller screens

*(Update this section as issues are identified and resolved in your specific build.)*

---

## 🤝 Contributing

Contributions are welcome! If you'd like to improve this project:

1. Fork the repository
2. Create a new branch (`git checkout -b feature/your-feature-name`)
3. Make your changes and commit (`git commit -m "Add: your feature description"`)
4. Push to your branch (`git push origin feature/your-feature-name`)
5. Open a Pull Request describing your changes

Please ensure any contributed code follows secure coding practices, especially around database queries and user input handling.

---

## 📄 License

This project is intended for **educational and academic purposes**. If you plan to reuse, modify, or distribute this project, please add an appropriate open-source license (e.g., MIT License) to clarify usage rights, or check with the original project author regarding licensing terms.

Example (MIT License placeholder):
```
MIT License

Copyright (c) [year] [author name]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software...
```

---

## 📬 Contact

For questions, suggestions, or issues related to this project:

- **GitHub:** [Your GitHub Profile Link]
- **Email:** [your-email@example.com]
- **Project Repository:** [Repository Link]

If you find this project useful, consider giving it a ⭐ on GitHub!

---

## 🙏 Acknowledgements

- Built using **PHP**, **MySQL**, and **Bootstrap**
- Inspired by real-world blood bank management challenges
- Thanks to the open-source community for tools like XAMPP/WAMP and phpMyAdmin that make local development simple

---

*This README was generated to provide comprehensive documentation for the Blood Bank & Donor Management System project. Update sections marked with placeholders (screenshots, contact info, license) with your actual project details before publishing.*

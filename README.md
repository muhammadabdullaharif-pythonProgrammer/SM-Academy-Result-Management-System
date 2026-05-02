# SM Academy — Result Management System

**Developer:** Muhammad Abdullah Arif — Frontend Web & Python Developer  
**CEO:** Sir Saad Manzoor SB  
**Version:** 1.0 | **Framework:** Django 4.2 | **Database:** SQLite (offline, no setup needed)

---

## Project Overview

SM Academy Result Management System is a complete web application built with Python and Django. It allows teachers to enter and manage student results, and students to view and download their result cards as PDF files. Everything runs 100% offline on your local machine using SQLite — no internet or external database needed.

---

## Features

### Teacher Features
- Secure login via Django Admin or main login page
- Add student results with subject-wise marks
- Subjects: Chemistry, Computer/Biology, TQ, IST, PST, Urdu, English, Math, Physics
- Edit total marks and obtained marks for any subject
- System auto-calculates:
  - Percentage
  - Grade (A+, A, B, C, F)
  - Remarks (Excellent, Very Good, Good, Needs Improvement, Fail)
- View all results in a clean dashboard table
- Delete results with confirmation

### Student Features
- Registration with: Class Name, Name, Father Name, Gender, Email, Password
- Login using username OR email
- Dashboard showing all personal result history
- Check result by entering name + roll number
- View full subject-wise result breakdown
- Download result as a professional PDF

---

## Installation & Setup (Step by Step)

### Step 1 — Make sure Python is installed
```
python --version
```
You need Python 3.9 or higher. Download from https://python.org if needed.

### Step 2 — Download / extract the project
Place the `smacademy` folder wherever you like, then open a terminal in that folder.

### Step 3 — Create a virtual environment (recommended)
```bash
python -m venv venv

# Windows:
venv\Scripts\activate

# Mac/Linux:
source venv/bin/activate
```

### Step 4 — Install required packages
```bash
pip install -r requirements.txt
```

### Step 5 — Set up the database
```bash
python manage.py migrate
```
This creates the `db.sqlite3` file automatically. No separate database software needed.

### Step 6 — Create demo accounts (optional but recommended)
```bash
python manage.py setup_demo
```
This creates:
- **Teacher account:** username=`teacher`, password=`teacher123`
- **Student account:** username=`student1`, password=`student123`

### Step 7 — Run the development server
```bash
python manage.py runserver
```

### Step 8 — Open in your browser
Go to: **http://127.0.0.1:8000/**

That's it! The system is fully running offline.

---

## How to Create Your Own Teacher Account

```bash
python manage.py createsuperuser
```
Enter a username, email, and password. Then log in to the Django Admin at:
**http://127.0.0.1:8000/admin/**

In the admin, open the user's record and set **Role = Teacher**.

---

## How a Teacher Adds Results

1. Log in at http://127.0.0.1:8000/login/ with the teacher account
2. Click **"Add New Result"** from the dashboard or navbar
3. Select a student from the dropdown (students who have registered appear here)
4. Enter Roll Number, Class, Exam Name, and Session
5. Fill in Total Marks and Obtained Marks for each subject
6. Click **Save Result**
7. The system automatically calculates percentage, grade, and remarks

**Grade Scale:**
| Percentage | Grade | Remarks              |
|-----------|-------|----------------------|
| 90% +     | A+    | Excellent            |
| 75–89%    | A     | Very Good            |
| 60–74%    | B     | Good                 |
| 45–59%    | C     | Needs Improvement    |
| Below 45% | F     | Fail                 |

---

## How a Student Checks Their Result

1. Register at http://127.0.0.1:8000/register/
2. Log in at http://127.0.0.1:8000/login/
3. From the dashboard, click **"Check My Result"**
4. Enter your full name and roll number
5. The complete result will appear on screen
6. Click **"Download PDF"** to save your result card

---

## Project Folder Structure

```
smacademy/
├── manage.py                   ← Run this to start the server
├── requirements.txt            ← Python packages needed
├── db.sqlite3                  ← Database (auto-created after migrate)
├── README.md                   ← This file
│
├── smacademy/                  ← Project settings
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── results/                    ← Main application
│   ├── models.py               ← Database tables
│   ├── views.py                ← Page logic
│   ├── urls.py                 ← URL routes
│   ├── forms.py                ← Form classes
│   ├── admin.py                ← Admin panel config
│   ├── pdf_generator.py        ← PDF result card builder
│   └── management/
│       └── commands/
│           └── setup_demo.py   ← Demo account creator
│
├── templates/                  ← HTML templates
│   ├── base.html               ← Shared layout (navbar, footer)
│   └── results/
│       ├── home.html
│       ├── register.html
│       ├── login.html
│       ├── teacher_dashboard.html
│       ├── student_dashboard.html
│       ├── check_result.html
│       ├── view_result.html
│       ├── add_result.html
│       ├── edit_result.html
│       ├── confirm_delete.html
│       └── student_list.html
│
└── static/                     ← CSS / JS files
    ├── css/
    └── js/
```

---

## Deployment Guide

### ⚠️ Important Note About Netlify
Netlify only hosts static websites (HTML/CSS/JS). Django is a Python backend — **it cannot be deployed on Netlify**. Use one of the platforms below instead.

---

### Option 1 — Render (Free, Recommended)

1. Push your project to GitHub
2. Go to https://render.com and sign up
3. Click **New → Web Service** and connect your GitHub repo
4. Set:
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn smacademy.wsgi:application`
5. Add environment variable: `SECRET_KEY` = (generate a new random key)
6. Also set `DEBUG = False` and update `ALLOWED_HOSTS` in settings.py

Add `gunicorn` and `whitenoise` to requirements.txt for production:
```
gunicorn
whitenoise
```

---

### Option 2 — PythonAnywhere (Free tier available)

1. Sign up at https://pythonanywhere.com
2. Upload your project files or clone from GitHub
3. Create a virtual environment and install requirements
4. Set up a Web App pointing to your project's wsgi.py
5. Full guide: https://help.pythonanywhere.com/pages/DeployExistingDjangoProject/

---

### Option 3 — Railway

1. Sign up at https://railway.app
2. Connect GitHub repo
3. Railway auto-detects Django and deploys it
4. Set environment variables (SECRET_KEY, DEBUG=False)

---

### Optional: Host only the frontend on Netlify
If you want to host a simple static landing page (like a redirect page or docs page) on Netlify, you can do that — but the actual Django backend still needs to run on Render, Railway, or PythonAnywhere. A static page can just link users to your Django app URL.

---

## Quick Reference

| URL                        | What it does                     |
|---------------------------|----------------------------------|
| /                          | Home page                        |
| /register/                 | Student registration             |
| /login/                    | Login (teacher or student)       |
| /dashboard/                | Teacher or student dashboard     |
| /add-result/               | Teacher: add new result          |
| /edit-result/<id>/         | Teacher: edit result             |
| /view-result/<id>/         | View full result card            |
| /check-result/             | Student: search by roll number   |
| /download-result/<id>/     | Download PDF result card         |
| /students/                 | Teacher: list all students       |
| /admin/                    | Django admin panel               |

---

*SM Academy Result Management System — Built with ❤️ by Muhammad Abdullah Arif*

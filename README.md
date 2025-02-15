# Securely Connecting MongoDB with Django using `.env`

This guide explains how to connect Django with MongoDB using `djongo`, ensuring secure storage of credentials using a `.env` file. 

## 🚀 Features
- Securely connect **MongoDB Atlas (Online)** and **Local MongoDB** with Django
- Hide sensitive credentials using `.env`
- Automatically load environment variables in `settings.py`

---

## 📦 Step 1: Install Required Libraries
Run the following command in your virtual environment:

```sh
pip install djongo pymongo dnspython python-dotenv
```

---

## 📂 Step 2: Create a `.env` File
Create a `.env` file in your **project root** (same directory as `manage.py`) and add:

```ini
MONGO_DB_NAME=djangocourses1
MONGO_URI=mongodb+srv://<your_username>:<your_password>@cluster0.mongodb.net/
MONGO_LOCAL_URI=mongodb://localhost:27017/djangocourses1
USE_ONLINE_DB=False  # Change to True when using MongoDB Atlas
```

> **🔒 Important:** Never push `.env` to GitHub! Add it to `.gitignore`:

```sh
echo ".env" >> .gitignore
```

---

## ⚙️ Step 3: Update `settings.py`
Modify your Django `settings.py` to load environment variables:

```python
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

# Use local or online MongoDB based on environment variable
USE_ONLINE_DB = os.getenv('USE_ONLINE_DB', 'False').lower() == 'true'

DATABASES = {
    'default': {
        'ENGINE': 'djongo',
        'NAME': os.getenv('MONGO_DB_NAME'),
        'CLIENT': {
            'host': os.getenv('MONGO_URI') if USE_ONLINE_DB else os.getenv('MONGO_LOCAL_URI'),
        }
    }
}
```

---

## 🌍 Step 4: Connect to MongoDB (Local & Online)

### 1️⃣ Run Django with Local MongoDB
- Start MongoDB locally:
  ```sh
  mongod --dbpath <your_mongo_data_directory>
  ```
- Run Django with local database:
  ```sh
  export USE_ONLINE_DB=False  # Mac/Linux
  set USE_ONLINE_DB=False  # Windows
  python manage.py runserver
  ```

### 2️⃣ Run Django with MongoDB Atlas (Online)
- Run Django with online database:
  ```sh
  export USE_ONLINE_DB=True  # Mac/Linux
  set USE_ONLINE_DB=True  # Windows
  python manage.py runserver
  ```

---

## 🚀 Step 5: Deploy & Push to GitHub
1. **Initialize a GitHub repository (if not done yet)**:
   ```sh
   git init
   git remote add origin <your_github_repo_url>
   ```
2. **Commit and push the safe code**:
   ```sh
   git add .
   git commit -m "Securely connected MongoDB with Django"
   git push origin main
   ```

---

## 🎯 Conclusion
Your Django app is now securely connected to **MongoDB**, and you can easily switch between **local and online databases** using environment variables. 🎉

<div align="center">

# 🔐 VaultLocker

### Your Personal Secure Password Manager

[![Python](https://img.shields.io/badge/Python-3.10+-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=for-the-badge&logo=flask&logoColor=white)](https://flask.palletsprojects.com)
[![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)](https://sqlite.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)
[![Status](https://img.shields.io/badge/Status-Active-brightgreen?style=for-the-badge)]()

<br/>

VaultLocker is a full-stack, security-focused password manager web application built with **Python Flask**, **SQLite**, **HTML/CSS**, and **JavaScript**. It uses **AES encryption** to store credentials safely, **OTP-based email authentication** for access control, and a clean, responsive UI with Light/Dark theme support.

<br/>

[📸 Screenshots](#-screenshots) · [🚀 Run Locally](#-getting-started) · [🔐 Security](#-security-overview) · [🗺️ Roadmap](#-roadmap)

</div>

---

## ✨ Features

| Feature | Description |
|---|---|
| 🔒 **AES Encryption** | All stored passwords are encrypted using the `cryptography.fernet` (AES-128) library before being saved to the database |
| 🧾 **OTP Authentication** | Login is protected by a 6-digit One-Time Password sent to the user's registered email, with a 5-minute expiry |
| 🚫 **Rate Limiting** | Login attempts are tracked and accounts are temporarily locked after 3 consecutive failures |
| 🌓 **Light / Dark Theme** | Full UI theme toggle with persistent preference via cookies |
| 👤 **Profile Management** | Upload profile picture, view account info, and manage account settings |
| ❌ **Account Deletion** | Users can permanently delete their account and all associated vault data |
| 🔑 **Password Reset** | Secure token-based password reset via email with approval/denial links and 1-hour token expiry |
| 📤 **Feedback System** | In-app feedback form that forwards messages to the developer's email |
| 🖥️ **Session Tracking** | Tracks last login time, session management, and supports graceful logout |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Frontend** | HTML5, CSS3, JavaScript (Vanilla) |
| **Backend** | Python 3.10+, Flask 2.x |
| **Database** | SQLite via Flask-SQLAlchemy |
| **Encryption** | Fernet (AES-128) — `cryptography` library |
| **Authentication** | Werkzeug password hashing (PBKDF2:SHA256), OTP via Flask-Mail |
| **Email** | Flask-Mail with Gmail SMTP (SSL, port 465) |
| **Security** | Flask-WTF CSRF protection, session-based auth |
| **Server** | Waitress (production WSGI server) |
| **Environment** | python-dotenv for secrets management |

---

## 📁 Project Structure

```
vaultlocker_web_app/
├── static/
│   ├── css/                   # Stylesheets
│   ├── js/                    # Frontend JavaScript
│   └── images/                # Screenshots and UI assets
│
├── templates/                 # Jinja2 HTML templates
│   ├── index.html             # Landing page
│   ├── login.html             # Login page
│   ├── register.html          # Registration page
│   ├── home.html              # Dashboard
│   ├── add_password.html      # Add new credential
│   ├── edit_password.html     # Edit existing credential
│   ├── view_passwords.html    # View vault entries
│   ├── otp_verification.html  # OTP verification step
│   ├── forgot_password.html   # Password reset page
│   ├── settings.html          # Account settings
│   ├── profile.html           # User profile
│   ├── about.html             # About page
│   ├── feedback.html          # Feedback form
│   └── privacy_policy.html    # Privacy policy
│
├── instance/                  # SQLite database (auto-generated, not committed)
│   └── xcrypt.db
│
├── app.py                     # Main Flask application (routes, models, logic)
├── generate_key.py            # One-time script to generate Fernet encryption key
├── requirements.txt           # Python dependencies
├── .env                       # Environment variables (NOT committed — see .env.example)
├── .env.example               # Template for required environment variables
├── encryption_key.key         # Fernet key (NOT committed — generated locally)
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites

- Python 3.10 or higher
- A Gmail account with an [App Password](https://myaccount.google.com/apppasswords) enabled
- Git

### Installation

**1. Clone the repository**

```bash
git clone https://github.com/sarveshdabke/vaultlocker_web_app.git
cd vaultlocker_web_app
```

**2. Create and activate a virtual environment**

```bash
# macOS / Linux
python -m venv venv
source venv/bin/activate

# Windows
python -m venv venv
venv\Scripts\activate
```

**3. Install dependencies**

```bash
pip install -r requirements.txt
```

**4. Set up environment variables**

```bash
cp .env.example .env
```

Open `.env` and fill in your values:

```env
EMAIL_USER=your_email@gmail.com
EMAIL_PASS=your_gmail_app_password
SECRET_KEY=your_strong_random_secret_key
```

> ⚠️ **Never commit your `.env` file.** It is already listed in `.gitignore`.

**5. Generate the encryption key**

```bash
python generate_key.py
```

> This creates `encryption_key.key` locally. Do not commit this file.

**6. Run the application**

```bash
python app.py
```

**7. Open your browser**

```
http://localhost:5000
```

---

## 📸 Screenshots

### 📝 Registration Page
![Registration Page](static/images/RegistrationPage.png)

---

### 🧭 Dashboard
![Dashboard](static/images/Dashboard.png)

---

### ➕ Add Password Page
![Add Password Page](static/images/AddPasswordPage.png)

---

### 🔐 View Password – Authentication Step
![View Password Authentication](static/images/ViewPasswordAuthentication.png)

---

### 👁️ View Password Page
![View Password Page](static/images/ViewPasswordPage.png)

---

### ⚙️ Settings Page
![Settings Page](static/images/SettingsPage.png)

---

## 🔐 Security Overview

VaultLocker is designed with a security-first mindset. Here is how each layer is protected:

| Threat | Mitigation |
|---|---|
| **Credential theft** | Passwords stored as AES-encrypted ciphertext using Fernet. Plaintext never written to disk. |
| **Account takeover** | OTP-based login: even with the correct password, access requires a 6-digit code sent to the registered email |
| **Brute force** | Login locked after 3 failed attempts with a 30-second cooldown timer |
| **Password reset abuse** | UUID tokens with 1-hour expiry; user must explicitly approve the reset from their inbox |
| **Weak master passwords** | User account passwords hashed with PBKDF2:SHA256 via Werkzeug |
| **Session forgery** | Flask sessions signed with a strong `SECRET_KEY` loaded from environment variables |
| **CSRF attacks** | Flask-WTF CSRF protection enabled on all state-changing routes |
| **Sensitive data exposure** | Environment variables loaded from `.env` (not committed); encryption key kept locally only |

---

## ⚙️ Environment Variables

Create a `.env` file in the project root based on `.env.example`:

```env
# Gmail address used to send OTPs and reset emails
EMAIL_USER=your_email@gmail.com

# Gmail App Password (NOT your Gmail login password)
# Generate one at: https://myaccount.google.com/apppasswords
EMAIL_PASS=your_app_password_here

# Strong random secret key for Flask session signing
# Generate with: python -c "import secrets; print(secrets.token_hex(32))"
SECRET_KEY=your_secret_key_here
```

---

## 📦 Dependencies

```
Flask
Flask-SQLAlchemy
Flask-Mail
Flask-WTF
Werkzeug
cryptography
python-dotenv
waitress
```

Install all with:

```bash
pip install -r requirements.txt
```

---

## 🗺️ Roadmap

- [x] AES-encrypted password storage
- [x] OTP-based email authentication
- [x] Login rate limiting and lockout
- [x] Token-based password reset
- [x] Profile picture upload
- [x] Light / Dark theme
- [x] Account deletion with cascade
- [ ] Built-in password strength checker
- [ ] Password generator tool
- [ ] Categories and tags for vault entries
- [ ] Export vault as encrypted PDF
- [ ] Browser extension for autofill
- [ ] Multi-factor authentication (TOTP)
- [ ] Cloud backup and restore

---

## 🤝 Contributing

Contributions are welcome! To contribute:

1. Fork the repository
2. Create a feature branch: `git checkout -b feat/your-feature-name`
3. Commit your changes: `git commit -m "feat: add your feature"`
4. Push to your branch: `git push origin feat/your-feature-name`
5. Open a Pull Request

Please follow [Conventional Commits](https://www.conventionalcommits.org/) for commit messages.

---

## 📄 License

This project is licensed under the **MIT License**.
See the [LICENSE](LICENSE) file for full details.

---

## 👤 Author

**Sarvesh C. Dabke**

[![GitHub](https://img.shields.io/badge/GitHub-sarveshdabke-181717?style=flat&logo=github)](https://github.com/sarveshdabke)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Sarvesh_Dabke-0077B5?style=flat&logo=linkedin)](https://www.linkedin.com/in/sarvesh-dabke-48437835a/)
[![Email](https://img.shields.io/badge/Email-dabkesarvesh7@gmail.com-D14836?style=flat&logo=gmail&logoColor=white)](mailto:dabkesarvesh7@gmail.com)

---

<div align="center">

⭐ If you found this project useful, please consider giving it a star — it helps others discover it!

</div>

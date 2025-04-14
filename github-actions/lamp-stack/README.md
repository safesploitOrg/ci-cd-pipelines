# ⚙️ GitHub Actions: LAMP Stack Setup

This GitHub Actions workflow installs and configures a complete **LAMP stack** (Linux, Apache, MySQL, PHP) on an Ubuntu runner.

It is designed for **CI/CD pipelines**, automated testing, and application bootstrapping in a safe and repeatable environment.

---

## 📁 Workflow Location

`.github/workflows/lamp-setup.yml`

---

## 🔧 What It Does

- Installs Apache, PHP, and MySQL (or MariaDB)
- Configures root MySQL password
- Creates a database and user using GitHub variables/secrets
- Imports a SQL schema file from `/db/testdb.sql`
- Verifies setup via SQL queries and HTTP requests

---

## 🛠 Requirements

### 🔐 GitHub Secrets (under **Settings > Secrets and variables > Actions > Secrets**)
| Secret Name           | Description                      |
|-----------------------|----------------------------------|
| `MYSQL_ROOT_PASSWORD` | Password for MySQL root user     |
| `MYSQL_PASSWORD`      | Password for custom DB user      |

### 🔧 GitHub Variables (under **Settings > Secrets and variables > Actions > Variables**)
| Variable Name    | Description             |
|------------------|-------------------------|
| `MYSQL_USER`     | Username to create      |
| `MYSQL_DATABASE` | Database to create/use  |

### 📦 SQL File

Place your SQL schema in the repo root:

/db/testdb.sql

This file includes placeholder values:
- `USERNAME_HERE`
- `PASSWORD_HERE`
- `DATABASE_NAME_HERE`

These will be replaced during the run using `sed`.

```bash
sed -i "s/USERNAME_HERE/${MYSQL_USER}/g" ./db/testdb.sql
sed -i "s/PASSWORD_HERE/${MYSQL_PASSWORD}/g" ./db/testdb.sql
sed -i "s/DATABASE_NAME_HERE/${MYSQL_DATABASE}/g" ./db/testdb.sql
```

---

## 📋 Output Summary

The workflow provides debugging:
- Logs of installed versions (Apache, PHP, MySQL)
- SQL output from `SHOW DATABASES`, `SHOW TABLES`, and `SELECT * FROM users;`
- Verification that Apache and PHP are serving properly via `curl`

---

## 🧪 Example Use Case

Ideal for:
- Testing LAMP-based apps like WordPress, Doogle, or custom PHP projects
- Automating the validation of SQL dumps and DB migrations
- Educational DevOps/DevSecOps pipelines

---

## 🧹 Notes

- This workflow uses `skip-grant-tables` temporarily to speed up configuration — not suitable for production use.
- Replace or harden MySQL auth steps for staging/prod environments.

---

## 📄 License

MIT License © safesploitOrg
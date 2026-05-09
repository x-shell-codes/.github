<div align="center">

<img src="https://raw.githubusercontent.com/x-shell-codes/.github/master/assets/x-shell-codes-logo.svg" width="280" alt="x-shell-codes" />

**Single-purpose shell scripts for Ubuntu server provisioning.**
Open the file. Read it. Run it. Move on.

[![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu_14.04%E2%80%9322.04-E95420?style=flat-square&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-CC_BY--SA_3.0-9333ea?style=flat-square)](https://creativecommons.org/licenses/by-sa/3.0/)

</div>

---

> ⚠️ **Server-only.** Every script in this org carries the same warning: **DO NOT RUN ON YOUR PC OR MAC.** These scripts disable SSH password auth, create system users, and install daemons — exactly the things you don't want on a laptop.

---

<table align="center"><tr><td align="center" width="33%">
<h3>🎯</h3><b>One concern per repo</b><br/><sub>A script does one thing</sub>
</td><td align="center" width="33%">
<h3>👀</h3><b>Read before you run</b><br/><sub>Short, intentionally readable</sub>
</td><td align="center" width="33%">
<h3>🐧</h3><b>Ubuntu-first</b><br/><sub>Tested 14.04 → 22.04</sub>
</td></tr></table>

---

## ⚡ How they work

```bash
wget https://raw.githubusercontent.com/x-shell-codes/<repo>/master/<repo>.sh
sudo bash <repo>.sh [flags]
```

For example, install Redis with a password:

```bash
wget https://raw.githubusercontent.com/x-shell-codes/redis/master/redis.sh
sudo bash redis.sh -p=mySecret
```

---

## 🏗️ Setting up a fresh server

A natural order. Run them top-to-bottom on a blank Ubuntu image.

```
1️⃣  Bootstrap          →  config · base · swap
2️⃣  Pick your runtime  →  php · nodejs · composer · puppeter
3️⃣  Add a database     →  mysql · mongodb · phpmyadmin
4️⃣  Cache & queue      →  redis · memcached
5️⃣  Web & TLS          →  nginx · ssl
6️⃣  Mail / proc / VCS  →  postfix · mailcatcher · supervisor · git
```

---

## 📚 Script catalog

### 🏗️ Bootstrap

| | Repo | What it does | Flags |
|---|---|---|---|
| 🛡️ | [**config**](https://github.com/x-shell-codes/config) | Disable SSH password auth, set hostname/timezone, create `deploy` user with sudo | – |
| 📦 | [**base**](https://github.com/x-shell-codes/base) | Install 28 essential packages (build-essential, python3, curl, zsh, jq…) | – |
| 💾 | [**swap**](https://github.com/x-shell-codes/swap) | Create swap file + tune `vm.swappiness` | – |

### 🌐 Web & TLS

| | Repo | What it does | Flags |
|---|---|---|---|
| 🚦 | [**nginx**](https://github.com/x-shell-codes/nginx) | Nginx + domain block + Certbot SSL | `-d`, `-s`, `-l` |
| 🔒 | [**ssl**](https://github.com/x-shell-codes/ssl) | Standalone Certbot SSL certificate | `-d`, `-s`, `-l` |

### 🛠️ Languages & runtimes

| | Repo | What it does | Flags |
|---|---|---|---|
| 🐘 | [**php**](https://github.com/x-shell-codes/php) | PHP install (default 8.1) + optional OPCache | `-p` |
| 🟢 | [**nodejs**](https://github.com/x-shell-codes/nodejs) | Node.js — choose source: APT / NodeSource / NVM | `-s` |
| 🎼 | [**composer**](https://github.com/x-shell-codes/composer) | PHP Composer + auto-update | – |
| 🎭 | [**puppeter**](https://github.com/x-shell-codes/puppeter) | Puppeteer for headless browsers *(repo name typo, not yours)* | – |

### 🗄️ Databases

| | Repo | What it does | Flags |
|---|---|---|---|
| 🐬 | [**mysql**](https://github.com/x-shell-codes/mysql) | MySQL + DBA password + remote toggle | `-p`, `-r` |
| 🍃 | [**mongodb**](https://github.com/x-shell-codes/mongodb) | MongoDB + password + remote toggle | `-p`, `-r` |
| 🖥️ | [**phpmyadmin**](https://github.com/x-shell-codes/phpmyadmin) | phpMyAdmin + admin password | `-p` |

### ⚡ Cache & queue

| | Repo | What it does | Flags |
|---|---|---|---|
| 🔴 | [**redis**](https://github.com/x-shell-codes/redis) | Redis + password protection + remote toggle | `-p`, `-r` |
| 💨 | [**memcached**](https://github.com/x-shell-codes/memcached) | Memcached + remote toggle | `-r` |

### 📧 Mail, processes, VCS

| | Repo | What it does | Flags |
|---|---|---|---|
| 📮 | [**postfix**](https://github.com/x-shell-codes/postfix) | Postfix MTA for outbound mail | `-d` |
| ✉️ | [**mailcatcher**](https://github.com/x-shell-codes/mailcatcher) | MailCatcher for staging/dev | – |
| 👁️ | [**supervisor**](https://github.com/x-shell-codes/supervisor) | Supervisor process control | – |
| 🌳 | [**git**](https://github.com/x-shell-codes/git) | Git + global user.name / user.email | `-u`, `-e` |

---

<details>
<summary><b>📐 Conventions</b></summary>

<br/>

- **One concern per repo** — a script does one thing
- **Read before you run** — they make root-level changes; the source is short and intentionally readable
- **Ubuntu-first** — most cover 14.04 → 22.04, a few are 20.04 / 22.04 only
- **No hidden network calls** — anything fetched is visible in the script
- **Idempotent where possible** — re-running shouldn't make things worse

</details>

---

<div align="center">

### 🤝 Contributing & support

Issues and PRs welcome on every repo. Bug reports go on the relevant script, not here.

<br/>

[![GitHub](https://img.shields.io/badge/All_repos-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/orgs/x-shell-codes/repositories)
[![Email](https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:www@mehmetogmen.com.tr)

<sub>CC BY-SA 3.0 · Maintained by <b>Mehmet ÖĞMEN</b> · Attribution required in derivatives</sub>

</div>

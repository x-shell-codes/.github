# x-shell-codes

A small toolbox of single-purpose shell scripts for setting up Ubuntu servers — the kind of one-off tasks you do every time you provision a new VM, kept tidy, readable, and reusable.

[![License: CC BY-SA 3.0](https://img.shields.io/badge/License-CC_BY--SA_3.0-lightgrey.svg)](https://creativecommons.org/licenses/by-sa/3.0/)
[![Shell](https://img.shields.io/badge/Shell-Bash-4EAA25?logo=gnu-bash&logoColor=white)](https://www.gnu.org/software/bash/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-14.04%E2%80%9322.04-E95420?logo=ubuntu&logoColor=white)](https://ubuntu.com/)

---

## What this is

Every time you spin up a new server, you do roughly the same things: lock down SSH, create a deploy user, install a web server, drop in a database, configure SSL, set up a process manager. None of it is hard, but copy-pasting the same commands across servers gets old fast — and it's exactly the kind of work where a forgotten step quietly turns into a 2 AM incident.

This organization is a collection of tiny, focused shell scripts that each handle **one** of those tasks. Each script lives in its own repository so you can use just the parts you need. They're written to be readable: you should be able to open the `.sh` file and understand exactly what it's about to do to your server before you run it.

---

## How they work

Every script follows the same pattern. Download it, read it, run it with `sudo`:

```bash
wget https://raw.githubusercontent.com/x-shell-codes/<repo>/master/<repo>.sh
sudo bash <repo>.sh [flags]
```

For example, to install Redis with a password set:

```bash
wget https://raw.githubusercontent.com/x-shell-codes/redis/master/redis.sh
sudo bash redis.sh -p=mySecret
```

Most scripts accept short flags (`-p`, `-d`, `-r`, etc.) for things like passwords, domain names, or whether the service should be reachable remotely. Each repo's README documents its own flags.

> ⚠️ **Server-only.** Every script in this org carries the same warning at the top of its README: **"DO NOT RUN THIS SCRIPT ON YOUR PC OR MAC!"** These scripts make changes that make sense on a fresh Ubuntu server (disabling password SSH auth, creating system users, installing daemons system-wide) and that you absolutely do not want on your laptop.

---

## Setting up a fresh server

If you're starting from a blank Ubuntu image, there's a natural order. The org is organized loosely around it.

### 1. Bootstrap the box

These three are usually the first things you run:

- **[`config`](https://github.com/x-shell-codes/config)** — the very first step. Disables SSH password authentication, sets the hostname and timezone, creates a `deploy` user, and grants it sudo access. After this, you log in as `deploy` over an SSH key, not as `root` with a password.
- **[`base`](https://github.com/x-shell-codes/base)** — installs the 28 packages most other scripts (and most projects) depend on: `build-essential`, `gcc`, `g++`, `python3`, `curl`, `zip`, `unzip`, `zsh`, `jq`, and friends.
- **[`swap`](https://github.com/x-shell-codes/swap)** — creates a swap file with sensible defaults and tunes `vm.swappiness` and cache pressure. Useful on small VPS instances where RAM is tight.

### 2. Add the runtime you need

Pick the language stack the box is going to run:

- **[`php`](https://github.com/x-shell-codes/php)** — installs PHP (default 8.1; pass `-p=8.2` for a different version). OPCache is available as a separate optional step.
- **[`composer`](https://github.com/x-shell-codes/composer)** — PHP's package manager, with auto-update enabled.
- **[`nodejs`](https://github.com/x-shell-codes/nodejs)** — Node.js, with a `-s` flag to choose the install source: APT, NodeSource PPA, or NVM.
- **[`puppeter`](https://github.com/x-shell-codes/puppeter)** — Puppeteer for headless-browser work. *(Yes, the repo name is misspelled — it's not a typo on your end.)*

### 3. Pick a database

- **[`mysql`](https://github.com/x-shell-codes/mysql)** — installs MySQL, sets the DBA password (`-p=...`), optionally opens it to remote connections (`-r=true`).
- **[`mongodb`](https://github.com/x-shell-codes/mongodb)** — same shape as MySQL: password and remote-access flags.
- **[`phpmyadmin`](https://github.com/x-shell-codes/phpmyadmin)** — drops in phpMyAdmin against your existing MySQL.

### 4. Caching and queues

- **[`redis`](https://github.com/x-shell-codes/redis)** — Redis with password protection enabled by default. `-r=true` exposes it remotely.
- **[`memcached`](https://github.com/x-shell-codes/memcached)** — Memcached, with the same `-r` flag for remote access.

### 5. Web server and TLS

- **[`nginx`](https://github.com/x-shell-codes/nginx)** — installs Nginx and creates a server block in one shot. Pass `-d=example.com -s=api` and it sets up `api.example.com`, generates an SSL certificate via Certbot, and configures the right directory permissions.
- **[`ssl`](https://github.com/x-shell-codes/ssl)** — if you just need a Certbot-issued certificate without touching Nginx config, this is the standalone version. Same `-d` and `-s` flags.

### 6. Mail, processes, and the rest

- **[`postfix`](https://github.com/x-shell-codes/postfix)** — installs and configures Postfix as an outbound mail relay. Pass the domain with `-d=...`.
- **[`mailcatcher`](https://github.com/x-shell-codes/mailcatcher)** — for staging or development boxes where you want to capture outgoing mail instead of actually sending it.
- **[`supervisor`](https://github.com/x-shell-codes/supervisor)** — installs Supervisor, the process control tool you'll want for keeping queue workers and long-running scripts alive.
- **[`git`](https://github.com/x-shell-codes/git)** — installs Git and (with `-u=name -e=email`) sets the global user/email so you can commit from the server.

> Full list at [github.com/orgs/x-shell-codes/repositories](https://github.com/orgs/x-shell-codes/repositories).

---

## Conventions

Across every script in the org:

- **One concern per repo.** A script does one thing. If you need two things, you run two scripts.
- **Read before you run.** The scripts make changes to your system as `root`. Open the `.sh` file first — they're short and intentionally readable.
- **Ubuntu-first.** The scripts target Ubuntu. Most cover **14.04 through 22.04**, a few are limited to **20.04 / 22.04** — each repo's README states what's been tested.
- **No hidden network calls.** Anything fetched from the internet is visible in the script.
- **Idempotent where possible.** Re-running a script shouldn't put your server into a worse state than it started in.

---

## Contributing

Issues and pull requests are welcome on every repository. Bug reports and feature requests go on the relevant script's repo, not here. If you've improved a script and used it in your own setup — drop a note, the maintainer would like to hear about it (it's part of the license).

---

## License & attribution

All scripts in this organization are released under the **[Creative Commons Attribution-ShareAlike 3.0 Unported License](https://creativecommons.org/licenses/by-sa/3.0/)**.

The maintainer asks two things, both straightforward:

1. **Attribution** — keep the original credit to **Mehmet ÖĞMEN** in any derivative work.
2. **Notification** — if you improve a script, let him know. It's not a legal requirement, just a nice thing to do.

Security vulnerabilities should be reported to **[www@mehmetogmen.com.tr](mailto:www@mehmetogmen.com.tr)**.

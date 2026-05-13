# WordPress Local Setup Using Docker

## Overview

This guide explains how to run an existing WordPress project locally using Docker.

This setup includes:

- WordPress container
- MySQL container
- Custom port (`8888`)
- Old database import
- Existing themes/plugins testing
- Local URL configuration

---

# Prerequisites

Install:

- Docker Desktop  
  https://www.docker.com/products/docker-desktop/

- Enable WSL2 during installation

Verify installation:

```bash
docker version
docker ps
```

---

# Project Structure

Create a folder:

```text
wordpress-local/
 ├── docker-compose.yml
 ├── wordpress.sql
 ├── wordpress/
 └── db/
```

Where:

| File/Folder | Purpose |
|---|---|
| docker-compose.yml | Docker configuration |
| wordpress.sql | Old database backup |
| wordpress/ | WordPress files |
| db/ | MySQL internal storage |

---

# Step 1 — Create docker-compose.yml

Create:

```text
docker-compose.yml
```

Add:

```yaml
services:
  wordpress:
    image: wordpress:latest
    container_name: local_wordpress
    ports:
      - "8888:80"

    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: wppass
      WORDPRESS_DB_NAME: wpdb

    volumes:
      - ./wordpress:/var/www/html

    depends_on:
      - db

  db:
    image: mysql:5.7
    container_name: local_wordpress_db

    environment:
      MYSQL_DATABASE: wpdb
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: wppass
      MYSQL_ROOT_PASSWORD: rootpass

    volumes:
      - ./db:/var/lib/mysql
```

---

# Step 2 — Start Containers

Open terminal inside:

```text
wordpress-local
```

Run:

```bash
docker compose up -d
```

Verify:

```bash
docker ps
```

You should see:

```text
local_wordpress
local_wordpress_db
```

---

# Step 3 — Open WordPress

Open browser:

```text
http://localhost:8888
```

---

# Step 4 — Copy Old WordPress Files

Copy your old WordPress files into:

```text
wordpress/
```

Especially:

```text
wp-content/themes
wp-content/plugins
wp-content/uploads
```

---

# Step 5 — Configure wp-config.php

Open:

```text
wordpress/wp-config.php
```

Update database values:

```php
define('DB_NAME', 'wpdb');
define('DB_USER', 'wpuser');
define('DB_PASSWORD', 'wppass');
define('DB_HOST', 'db');
```

Important:

```php
DB_HOST = db
```

NOT:

```php
localhost
```

---

# Step 6 — Import Existing Database

Place:

```text
wordpress.sql
```

inside:

```text
wordpress-local/
```

## PowerShell Import Command

Run:

```powershell
Get-Content .\wordpress.sql | docker exec -i local_wordpress_db mysql -uwpuser -pwppass wpdb
```

## CMD Import Command

If using CMD:

```cmd
docker exec -i local_wordpress_db mysql -uwpuser -pwppass wpdb < wordpress.sql
```

---

# Step 7 — Update Local URLs

Open MySQL shell:

```powershell
docker exec -it local_wordpress_db mysql -uwpuser -pwppass wpdb
```

Run:

```sql
UPDATE wp_options
SET option_value='http://localhost:8888'
WHERE option_name IN ('siteurl','home');
```

Exit:

```sql
exit;
```

---

# Step 8 — Restart Containers

```bash
docker compose restart
```

---

# Step 9 — Access Site

Frontend:

```text
http://localhost:8888
```

Admin:

```text
http://localhost:8888/wp-admin
```

---

# Useful Docker Commands

## Start

```bash
docker compose up -d
```

## Stop

```bash
docker compose down
```

## Restart

```bash
docker compose restart
```

## View Logs

```bash
docker compose logs -f
```

## WordPress Logs

```bash
docker compose logs wordpress
```

## Database Logs

```bash
docker compose logs db
```

---

# Common Issues

## 1. ERR_EMPTY_RESPONSE

Restart containers:

```bash
docker compose down
docker compose up -d
```

---

## 2. Error Establishing Database Connection

Verify:

```php
define('DB_HOST', 'db');
```

and not:

```php
localhost
```

---

## 3. PowerShell `<` Error

Use:

```powershell
Get-Content .\wordpress.sql | docker exec -i local_wordpress_db mysql -uwpuser -pwppass wpdb
```

---

## 4. Port Already In Use

Change:

```yaml
- "8888:80"
```

to another port:

```yaml
- "9090:80"
```

---

# Recommended Versions for Old Projects

For better compatibility with older WordPress sites:

```yaml
wordpress:6.4
mysql:5.7
```

---

# Final Notes

This setup is ideal for:

- Testing old WordPress projects
- Plugin development
- Theme debugging
- Migration validation
- Local development without XAMPP

Advantages over XAMPP:

- Cleaner isolation
- Easy resets
- Portable setup
- Multiple projects support
- No Apache/PHP conflicts
- Version control friendly

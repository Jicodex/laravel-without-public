# 🚀 Run Laravel Without `/public` in URL (Easy & Safe Method)

This guide shows an **easy, safe, and beginner‑friendly way** to run a Laravel project **without `/public` in the URL**, **without deleting or moving the original `public` folder**.

👉 We only **COPY** files — nothing is removed.

Final URL:

```
https://yourdomain.com
```

---

## ✅ Key Rules (Important)

* ❌ Do NOT delete the `public` folder
* ❌ Do NOT move `public` folder
* ✅ COPY all files & folders **from `public`** to **root (same level as `app`, `routes`, etc.)**


This works perfectly on shared hosting / cPanel.

---

## 📂 Final Folder Structure

```
/your-domain
│
├── app/
├── bootstrap/
├── config/
├── database/
├── resources/
├── routes/
├── storage/
├── vendor/
├── public/          (original public folder — untouched)
│
├── css/             (copied from public)
├── js/              (copied from public)
├── images/          (copied from public)
├── assets/          (copied from public)
├── index.php        (copied from public)
├── .htaccess        (copied from public)
│
├── .env
└── artisan
```

---

## 🛠 Step‑by‑Step (Easy Way)

### 1️⃣ Upload Full Laravel Project

Upload your **entire Laravel project** to your domain root:

```
/your-domain
```

Do NOT remove the `public` folder.

---

### 2️⃣ COPY Files From `public`

From:

```
/public
```

COPY all files & folders:

```
index.php
.htaccess
css/
js/
images/
assets/
```

PASTE them directly into:

```
/your-domain
```

✅ After copying, **the same files will exist in two places** — this is expected.

---

### 3️⃣ Update Root `index.php`

Open the **copied** `index.php` (root level) and set paths like this:

```php
<?php

use Illuminate\Foundation\Application;
use Illuminate\Http\Request;

define('LARAVEL_START', microtime(true));

// Determine if the application is in maintenance mode...
if (file_exists($maintenance = __DIR__.'/storage/framework/maintenance.php')) {
    require $maintenance;
}

// Register the Composer autoloader...
require __DIR__.'/vendor/autoload.php';

// Bootstrap Laravel and handle the request...
/** @var Application $app */
$app = require_once __DIR__.'/bootstrap/app.php';

$app->handleRequest(Request::capture());
```

✔ Paths now point correctly to Laravel core.

---

### 4️⃣ Root `.htaccess`

Use this `.htaccess` in the root directory:

```apache
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews -Indexes
    </IfModule>

    RewriteEngine On

    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    # Handle X-XSRF-Token Header
    RewriteCond %{HTTP:x-xsrf-token} .
    RewriteRule .* - [E=HTTP_X_XSRF_TOKEN:%{HTTP:X-XSRF-Token}]

    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]

    # Send Requests To Front Controller...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^(.*)$ public/$1 [L]
</IfModule>
```

---

### 5️⃣ Update `.env`

```env
APP_URL=https://yourdomain.com
```

Make sure database & mail configs are correct.

---

### 6️⃣ Permission Fix

Set permission for:

```
storage/
bootstrap/cache/
```

Recommended:

```
755 or 775
```

---

## 🎯 Result

Your Laravel project will now open at:

```
https://yourdomain.com
```

✅ No `/public` in URL
✅ Original `public` folder still exists
✅ Safe & reversible

---

## ⚠️ Common Problems

### 500 Error

* Check `.htaccess`
* Check file permission
* Check correct `index.php` paths

### Asset Not Loading

* Make sure `css/js/images` are copied correctly

---

## ❌ When NOT to Use This

* VPS / Cloud hosting
* Hosting allows document‑root change

👉 In those cases, point domain directly to `/public`.

---

## 🔍 SEO Keywords / Tags

```
laravel hosting
laravel without public
remove public from laravel url
laravel root index.php
laravel shared hosting
laravel cpanel deploy
laravel production setup
laravel htaccess
```

---

## ⭐ Support

If this helped you, please ⭐ **star the repository** so other developers can find it easily.

Happy Laravel Coding 🚀

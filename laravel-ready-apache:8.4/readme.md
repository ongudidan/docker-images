

### **Docker Hub Image Name**

```
ongudidan/laravel-ready-apache:8.4
```

---

### **Apache Configuration**

This image comes with two Apache configurations:

1.  **HTTPS (Default)**: `laravel-apache-https.conf` - Enforces HTTPS redirection using `X-Forwarded-Proto`.
    - **Cloud Run Compatible**: Automatically sets `HTTPS=on` environment variable when behind a proxy, ensuring Laravel generates HTTPS URLs for assets.
2.  **HTTP**: `laravel-apache-http.conf` - Standard HTTP configuration (useful for local dev without SSL proxy).

To switch configurations, update the `COPY` command in your `Dockerfile`:

**For HTTPS (Default):**
```dockerfile
COPY laravel-apache-https.conf /etc/apache2/sites-available/000-default.conf
```

**For HTTP:**
```dockerfile
COPY laravel-apache-http.conf /etc/apache2/sites-available/000-default.conf
```

---

### **Laravel Configuration (Required for Cloud Run)**

> [!IMPORTANT]
> When deploying to Cloud Run or any reverse proxy environment, you **must** configure Laravel's `TrustProxies` middleware to trust proxy headers. Without this, Laravel will generate HTTP URLs instead of HTTPS URLs for assets.

**Edit `app/Http/Middleware/TrustProxies.php` in your Laravel application:**

```php
<?php

namespace App\Http\Middleware;

use Illuminate\Http\Middleware\TrustProxies as Middleware;
use Illuminate\Http\Request;

class TrustProxies extends Middleware
{
    /**
     * The trusted proxies for this application.
     * Trust all proxies for Cloud Run
     */
    protected $proxies = '*';

    /**
     * The headers that should be used to detect proxies.
     */
    protected $headers =
        Request::HEADER_X_FORWARDED_FOR |
        Request::HEADER_X_FORWARDED_HOST |
        Request::HEADER_X_FORWARDED_PORT |
        Request::HEADER_X_FORWARDED_PROTO;
}
```

**Why this is needed:** Cloud Run terminates HTTPS at the load balancer and forwards requests as HTTP with `X-Forwarded-Proto: https` header. Laravel needs to trust this header to generate HTTPS URLs.

---

### **Steps to Build and Push**

```bash
# 1. Go to your repo
cd /path/to/your/repo

# 2. Build the image
docker build -t ongudidan/laravel-ready-apache:8.4 .

# 3. Test locally with any Laravel project
docker run -it --rm -p 8000:80 -v /path/to/laravel/project:/app ongudidan/laravel-ready-apache:8.4

# 4. Log in to Docker Hub
docker login

# 5. Push the image
docker push ongudidan/laravel-ready-apache:8.4

# 6. Use anywhere by pulling and mounting any Laravel project
docker run -it --rm -p 8000:80 -v /path/to/new/laravel/project:/app ongudidan/laravel-ready-apache:8.4
```

---

✅ **Result:**

* Out-of-the-box Laravel-ready container.
* Apache configured for `/app/public` with easy HTTP/HTTPS switching.
* `mod_rewrite` enabled by default.
* Reusable for **any Laravel project**.


---


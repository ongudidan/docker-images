

### **Docker Hub Image Name**

```
ongudidan/laravel-ready-apache:8.4
```

---

### **Steps to Build and Push**

```bash
# 1. Go to your repo with Dockerfile and laravel-apache.conf
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
* Apache already configured for `/app/public`.
* Reusable for **any Laravel project**.

---


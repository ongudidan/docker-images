# **Custom Docker Images for PHP & Laravel Development**

This repository contains **pre-built and reusable custom Docker images** for PHP and Laravel development. Each image is configured to work **out-of-the-box**, so you can start new projects quickly without spending time on Apache/Nginx or PHP configuration.

It is **public** and designed for developers who want ready-to-use images or want to build and push their own custom images using these folders as templates.

---

## **Overview**

* Each folder in this repo represents a **different custom Docker image**.
* Images include pre-configured environments such as:

  * Laravel-ready Apache containers
  * PHP-FPM with Nginx
  * Any other custom PHP environment you want to add
* New images can be added over time, each in its own folder with a Dockerfile and supporting configuration files.

---

## **Repository Structure**

```
.
├── laravel-ready-apache/
│   ├── Dockerfile
│   └── laravel-apache.conf
├── php8.3-fpm-nginx/
│   ├── Dockerfile
│   └── nginx.conf
└── README.md
```

* Each folder contains all files needed to **build the specific Docker image**.
* Follow the same structure when adding new images.

---

## **Using the Images**

### **1. Pull a pre-built image from Docker Hub**

```bash
docker pull ongudidan/laravel-ready-apache:8.4
docker pull ongudidan/php8.3-fpm-nginx:latest
```

### **2. Run a container with your Laravel project**

```bash
docker run -it --rm -p 8000:80 -v /path/to/your/laravel/project:/app ongudidan/laravel-ready-apache:8.4
```

* Apache serves `/app/public`.
* You can mount **any Laravel project** into `/app`.

### **3. Build your own image from this repository**

```bash
cd laravel-ready-apache
docker build -t ongudidan/laravel-ready-apache:8.4 .

cd ../php8.3-fpm-nginx
docker build -t ongudidan/php8.3-fpm-nginx:latest .
```

---

## **Build & Push Your Custom Docker Images**

This workflow is **universal for all folders** in the repository.

### **1. Log in to Docker Hub**

**Option 1: Standard login**

```bash
docker login
```

**Option 2: Using Docker Hub username + access token**

```bash
docker login -u <your-dockerhub-username>
# Enter your personal access token as password
```

### **2. Navigate to the folder of the image you want to build**

```bash
cd <folder-name>
```

* Example:

```bash
cd laravel-ready-apache
```

### **3. Build the Docker image**

```bash
docker build -t <your-dockerhub-username>/<image-name>:<tag> .
```

* Example:

```bash
docker build -t ongudidan/laravel-ready-apache:8.4 .
```

### **4. Test the image locally**

```bash
docker run -it --rm -p 8000:80 -v /path/to/your/project:/app <your-dockerhub-username>/<image-name>:<tag>
```

* Example:

```bash
docker run -it --rm -p 8000:80 -v ~/projects/my-laravel:/app ongudidan/laravel-ready-apache:8.4
```

### **5. Push the image to Docker Hub**

```bash
docker push <your-dockerhub-username>/<image-name>:<tag>
```

* Example:

```bash
docker push ongudidan/laravel-ready-apache:8.4
```

### **6. Use the image anywhere**

```bash
docker run -it --rm -p 8000:80 -v /path/to/project:/app <your-dockerhub-username>/<image-name>:<tag>
```

---

## **Contributing**

Contributions are welcome!

* To add a new image:

  1. Create a new folder with:

     * `Dockerfile`
     * any custom configuration files
  2. Update this README with a description of the new image.
  3. Submit a pull request.

* Follow the **existing folder structure** to maintain consistency.

---

## **Notes**

* Always mount your project folder at `/app` for Laravel images.
* Use tags to version your images (e.g., `:v1.0`, `:latest`).
* Remove old local images if needed:

```bash
docker rmi <image-name>:<tag>
```

---

## **License**

This repository is public and free to use. Modify and redistribute as needed.

---

## **Support / Contact**

Open an issue on GitHub if you encounter problems or have questions.

---

This README is now **fully comprehensive, professional, and ready to share publicly**.

It shows:

* Repo overview
* Current images
* Folder structure
* How to use, build, and push any image
* Docker Hub login with username or access token
* Contribution instructions

---

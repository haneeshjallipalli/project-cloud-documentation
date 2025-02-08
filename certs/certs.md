### **🚀 Step-by-Step Guide: Installing Certbot & Setting Up SSL for `example.cloud`**  

Since you are using **Certbot** for real SSL certificates (Let's Encrypt), follow these steps to install, generate, and configure your SSL certificates correctly.

---

## **1️⃣ Install Certbot on Ubuntu (for Let's Encrypt)**
Run the following commands to install Certbot and Nginx plugin:
```bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
```
This installs **Certbot**, which will automatically obtain and renew SSL certificates.

---

## **2️⃣ Obtain SSL Certificates for `example.cloud`**
Run this command to generate a **free Let's Encrypt SSL certificate**:
```bash
sudo certbot certonly --standalone -d example.cloud
```
- **`certonly`** → Only get the certificate (we manually configure Nginx).
- **`--standalone`** → Uses a temporary web server to verify your domain.
- **`-d example.cloud`** → Replace with your actual domain.

📌 **IMPORTANT:** Your domain **must point to your EC2 instance** before running this command.

---

## **3️⃣ Copy Certificates to Your `certs/` Directory**
After obtaining the certificate, Certbot stores it in:
```
/etc/letsencrypt/live/example.cloud/
```
Now, copy the required files to your project’s `certs/` folder:
```bash
mkdir -p certs
sudo cp /etc/letsencrypt/live/example.cloud/fullchain.pem certs/example.cloud.crt
sudo cp /etc/letsencrypt/live/example.cloud/privkey.pem certs/example.cloud.key
```

**Set correct permissions:**
```bash
sudo chmod 644 certs/example.cloud.crt
sudo chmod 600 certs/example.cloud.key
```

---

## **4️⃣ Update `nginx.conf` to Use Let's Encrypt SSL**
Modify your `nginx.conf` file to use the newly obtained **real SSL certificates**:

```nginx
server {
    listen 80;
    server_name example.cloud;

    # Redirect all HTTP requests to HTTPS
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name example.cloud;

    ssl_certificate /etc/nginx/certs/example.cloud.crt;
    ssl_certificate_key /etc/nginx/certs/example.cloud.key;

    location / {
        proxy_pass http://example.cloud:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

---

## **5️⃣ Restart Nginx to Apply Changes**
```bash
docker restart nginx-server
```
OR if not using Docker yet:
```bash
sudo systemctl restart nginx
```

---

## **6️⃣ Enable Automatic SSL Certificate Renewal**
To ensure your SSL certificate **renews automatically**, set up a cron job:
```bash
sudo crontab -e
```
Add this line to renew the certificate automatically every 3 months:
```
0 3 * * * certbot renew --quiet --post-hook "docker restart nginx-server"
```
- `0 3 * * *` → Runs at **3 AM every day**.
- `certbot renew --quiet` → Renews certificates if needed.
- `--post-hook "systemctl reload nginx"` → Restarts Nginx to apply the new certificate.

---

## **🔥 Final Steps: Restart & Verify Everything**
1️⃣ **Restart Docker Services**
```bash
docker-compose down --rmi all --volumes --remove-orphans
docker-compose up -d --build
```

2️⃣ **Verify SSL Is Working**
Check the SSL certificate details in the browser by visiting:
```
https://example.cloud
```

3️⃣ **Manually Test SSL Renewal**
Run:
```bash
sudo certbot renew --dry-run
```
If successful, it means your SSL certificate will renew automatically.

---

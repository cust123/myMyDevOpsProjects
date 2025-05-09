
# 🚀 DevOps Project: Deploy a Secure Web Page on AWS EC2 with Apache, Route 53, and SSL

This guide walks you through deploying a simple website on an AWS EC2 instance using:

- ✅ Ubuntu Server
- ✅ Apache Web Server
- ✅ Domain from Namecheap
- ✅ DNS via Route 53
- ✅ Secure HTTPS with Let's Encrypt (Certbot)

---

## 🔧 Prerequisites

- AWS account
- Namecheap domain
- A terminal with SSH and SCP support (e.g. Git Bash, Linux shell)
- `.pem` key file for EC2 access
- Basic HTML file (e.g., `index.html`)

---

## 🖥️ Step 1: Launch an EC2 Instance

1. Open AWS Console → EC2 → Launch instance
2. Choose:
   - **AMI**: Ubuntu Server (20.04 or 22.04)
   - **Instance type**: t2.micro (free tier)
   - **Key pair**: Create or use existing `.pem` key
   - **Security group**:
     - Allow **HTTP (80)**
     - Allow **HTTPS (443)**
     - Allow **SSH (22)**
3. Launch the instance

---

## 🔑 Step 2: Connect to EC2 via SSH

```bash
ssh -i /path/to/your-key.pem ubuntu@your-ec2-public-ip
```

---

## 🌐 Step 3: Install Apache Web Server

```bash
sudo apt update -y
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
```

Test in your browser:

```
http://your-ec2-public-ip
```

---

## 🌍 Step 4: Configure Domain with Route 53 and Namecheap

### A. Create Hosted Zone in Route 53

1. Open Route 53 → Hosted zones → Create hosted zone
2. Enter your domain name (e.g., `yourdomain.com`)
3. Note the NS (Name Server) records

### B. Update Namecheap DNS

1. Log in to Namecheap → Domain List → Manage your domain
2. Set **Custom DNS** using the 4 NS records from Route 53

### C. Add A Record in Route 53

1. In your hosted zone → Create record
2. Record type: **A – IPv4**
3. Value: EC2 Public IP
4. Name: Leave blank (for root domain)

Test in browser:

```
http://yourdomain.com
```

---

## 📤 Step 5: Upload Custom HTML Page

From your local machine:

```bash
scp -i /path/to/your-key.pem /path/to/index.html ubuntu@yourdomain.com:/tmp
```

Then on EC2:

```bash
sudo mv /tmp/index.html /var/www/html/index.html
```

---

## 🔒 Step 6: Secure Your Site with HTTPS using Certbot

### A. Install Certbot

```bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
```

### B. Run Certbot

```bash
sudo certbot --apache
```

Follow prompts to:
- Enter email
- Agree to terms
- Choose your domain

Test in browser:

```
https://yourdomain.com
```

---

## 🔁 (Optional) Step 7: Test Auto-Renewal

```bash
sudo certbot renew --dry-run
```

---

## ✅ Success! You’ve Deployed a Secure Website with:

- EC2 Hosting
- Apache Web Server
- Route 53 DNS
- Namecheap Domain
- HTTPS with Certbot

---

### 🧩 Next Steps (Optional Ideas)

- Automate deployment with GitHub Actions + EC2
- Set up monitoring with CloudWatch
- Add CI/CD using Jenkins or GitHub Actions

---

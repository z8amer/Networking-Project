# 🌐 AWS Networking Project: EC2 + Domain + NGINX

## 📋 Project Overview
I built and deployed a live web server on AWS EC2 and connected it to my custom domain (zain-amer.co.uk) using Cloudflare DNS.

## 🏗️ Architecture
- **AWS EC2** (t3.micro) running Ubuntu 24.04
- **NGINX** web server
- **Elastic IP** for static addressing
- **Cloudflare** for DNS management and SSL

## 📸 Screenshots

### EC2 Instance Running
![EC2 Instance](./screenshots/ec2-instance.png)

### Security Group Rules
![Security Group](./screenshots/security-group.png)

### Nginx Working via IP
![Nginx via IP](./screenshots/nginx-ip.png)

### Domain Live!
![Domain Live](./screenshots/domain-live.png)

### Cloudflare DNS Configuration
![Cloudflare DNS](./screenshots/cloudflare-dns.png)

### SSL/TLS Setting (Flexible)
![Cloudflare SSL](./screenshots/ssl-flexible.png)

## 💻 Commands I Used

### Connecting to EC2

ssh -i your-key.pem  your-instance-ip


### Installing NGINX

```bash
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
```

### Testing the Server

curl http://localhost
#### Also tested in browser via IP and domain


## 🧠 What I Learned

### 1. Always Check Which Machine You're On!
I kept getting errors because I was typing local file paths while connected to my EC2 instance.

| Prompt | Meaning |
|--------|---------|
| `ubuntu@ip-172-xx:~$` | **I'm on the cloud server** (EC2) |
| `C:\Users\Zain.Amer>` | **I'm on my local machine** |

### 2. Security Groups Are Like Bouncers
- **Port 22 (SSH):** Only open to MY IP address (security)
- **Port 80 (HTTP):** Open to everyone (0.0.0.0/0) so the world can see my site

### 3. The Elastic IP
When I attached an Elastic IP to my instance, I thought it would keep the original IP AND add a new permanent one. **Wrong!** The Elastic IP **REPLACES** the original IP completely. My old IP was released back to AWS.

### 4. DNS Doesn't Update Automatically
After attaching my Elastic IP, my domain still didn't work. Why? Because Cloudflare was still pointing to my old IP! I had to manually update the A record.



## 🚧 Challenges I Faced

### Challenge 1: SSH Connection Timed Out
**Problem:** Suddenly couldn't connect to my instance

**Solution:** Checked security group - my IP had changed! Updated the SSH rule to allow my new IP.

### Challenge 2: Cloudflare 522 Error
**Problem:** Got "Connection timed out" when visiting my domain

**Solution:** My DNS record was Proxied (orange cloud) but pointing to the wrong IP. Updated to correct Elastic IP.

### Challenge 3: Terminal Environment Confusion
**Problem:** Tried to access my Windows Downloads folder while SSH'd into Linux

**Solution:** Learned to read the prompt! `ubuntu@...` means I'm in the cloud, not on my local machine.

### Challenge 4: Domain Not Working After Elastic IP
**Problem:** Site worked via IP but not domain

**Solution:** Realized DNS still pointed to old IP. Updated Cloudflare A record to new Elastic IP.

### Challenge 5: Cloudflare 522 Error After DNS Update
**Problem:** After pointing my domain to the correct Elastic IP, I still got a 522 "Connection timed out" error when visiting `http://zain-amer.co.uk`. The IP worked fine directly.

**Solution:** The issue was the Cloudflare SSL/TLS setting. My server only speaks HTTP (no SSL certificate), but Cloudflare was set to "Full" or "Full (strict)", meaning it tried to connect to my server over HTTPS. Changing the SSL/TLS setting to **"Flexible"** fixed it—Cloudflare now talks to my server over plain HTTP while still offering HTTPS to visitors. This is a quick fix; for production, I would install an SSL certificate on the server and use "Full (strict)" for end-to-end encryption.


## ✅ Final Working Setup
- **Elastic IP:** 16.170.80.62
- **Domain:** zain-amer.co.uk
- **Status:** LIVE! 🎉

Visit http://zain-amer.co.uk to see it working.

## 📅 Date Completed
February 23, 2026

## 🔗 Resources
- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [Cloudflare DNS Guide](https://developers.cloudflare.com/dns/)
- [NGINX Beginner's Guide](https://nginx.org/en/docs/beginners_guide.html)

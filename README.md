# DevOps Assignment – ARTH

## 👨‍💻 Environment

* AWS EC2 Instance (Amazon Linux)
* Public IP used for testing
* Tools: Docker, Nginx, Linux

---

# ✅ Task 1 – Basic Linux Setup

## 🔹 User Creation & Sudo Access

```bash
sudo adduser devopsuser
sudo passwd devopsuser
sudo usermod -aG wheel devopsuser
id devopsuser
```

![User Creation](https://github.com/user-attachments/assets/18056690-cef6-45af-8f36-18faa8c699c6)

---

## 🔹 Package Installation

```bash
sudo yum update -y
sudo yum install -y git curl htop nginx docker lsof
```

![Package Installation](https://github.com/user-attachments/assets/55464099-b8f8-41cb-8225-f0a8a81a6046)

---

## 🔹 Start Docker

```bash
sudo systemctl start docker
sudo systemctl enable docker
```
![Docker Installation](https://github.com/user-attachments/assets/abd2e291-7dc6-45a2-a7e4-82b4cb362f4e)

---

## 🔹 System Information

```bash
cat /etc/os-release
ip a
free -h
df -h
```

![OS Version](https://github.com/user-attachments/assets/cc481ece-5958-4cb8-ab5c-807aa3637890)
![IP Address](https://github.com/user-attachments/assets/1483c762-fc35-42bb-b449-7d1a6fe5cbb7)
![Memory Usage](https://github.com/user-attachments/assets/370d404f-4302-4e04-98bb-da05250f384d)
![Disk Usage](https://github.com/user-attachments/assets/77f6f2d4-b2a3-47ae-b702-60d6873475bb)

---

# ✅ Task 2 – Service Management

## 🔹 Start & Enable Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

## 🔹 Check Status

```bash
sudo systemctl status nginx
```

![Nginx Status](https://github.com/user-attachments/assets/da954243-4389-448a-b9e9-a720df786b36)

---

## 🔹 Check Port Usage

```bash
sudo lsof -i :80
```

![Port Check](https://github.com/user-attachments/assets/0e279c8d-a5a1-4ac3-b778-21532f1eacaf)

---

# ✅ Task 3 – Docker Web Application

## 🔹 Create Project

```bash
mkdir docker-app
cd docker-app
```

---

## 🔹 index.html

```html
<h1>Hello from DevOps Assignment 🚀</h1>
```

---

## 🔹 Dockerfile

```dockerfile
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
```

![Dockerfile](https://github.com/user-attachments/assets/9d5f0e37-df0c-474d-9b72-2add4f0df286)

---

## 🔹 Build & Run Container

```bash
sudo docker build -t myapp .
sudo docker run -d -p 8080:80 myapp
```

---

## 🔹 Verify Application

```bash
curl http://localhost:8080
```
## 🔹 Browser Access

```
http://<public-ip>:8080
```

![Docker Output](https://github.com/user-attachments/assets/5882515c-98d2-4b3f-98c2-5f14f6e0a81e)

---

![Browser Output](https://github.com/user-attachments/assets/your-screenshot11)

---

# ✅ Task 4 – Nginx Reverse Proxy

## 🔹 Configure Nginx

```bash
sudo nano /etc/nginx/conf.d/default.conf
```

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://localhost:8080;
    }
}
```

---

## 🔹 Restart Nginx

```bash
sudo systemctl restart nginx
```

---

## 🔹 Verify Reverse Proxy

```bash
curl http://localhost
```

![Nginx Curl](https://github.com/user-attachments/assets/0cb34c68-712c-4ece-98ae-425281b74073)

---

## 🔹 Browser Access

```
http://<public-ip>
```

![Nginx Browser](https://github.com/user-attachments/assets/08bae0ea-3f19-4077-b554-5fc40036f20e)

---

# ✅ Task 5 – Troubleshooting

## 🔹 Issue

Application was not accessible using public IP on port 8080.

---

## 🔹 Diagnosis

* Verified container running using:

```bash
sudo docker ps
```

* Localhost worked but public access failed.

---

## 🔹 Root Cause

Port 8080 was not allowed in EC2 Security Group.

---

## 🔹 Fix Applied

Added inbound rule:

* Type: Custom TCP
* Port: 8080
* Source: 0.0.0.0/0
![custom rule](https://github.com/user-attachments/assets/eed60857-194a-4fc1-9e78-8a641ea146fa)
---

## 🔹 Verification

* Application successfully accessed via:

```
http://<public-ip>:8080
```

![Before Fix](https://github.com/user-attachments/assets/c2dfcd16-4c63-4f46-8b09-8f2f03dfc4ed)
![After Fix](https://github.com/user-attachments/assets/60e5f36c-471c-470f-996a-8097aca1fff7)

---

# ✅ Task 6 – Shell Script

## 🔹 Script File (check.sh)

```bash
#!/bin/bash

echo "Disk Usage:"
df -h

echo "Memory Usage:"
free -h

echo "Nginx Status:"
systemctl status nginx | grep Active

echo "Port 8080 Listening:"
ss -tuln | grep 8080
```

---

## 🔹 Execute Script

```bash
chmod +x check.sh
./check.sh
```

![Script Output](https://github.com/user-attachments/assets/fb46d7bc-4ac2-4a9e-89f3-db79a0951578)

---

# ✅ Task 7 – Short Answers

**1. Docker Image vs Container**

Image is a template; container is a running instance of that image.

**2. systemctl start vs enable**

Start runs service immediately; enable starts it at boot.

**3. Nginx Reverse Proxy**

It forwards client requests to backend servers.

**4. Check process using port**

Use: `lsof -i :PORT`

**5. AWS EC2**

Virtual server in the cloud.

**6. Jenkins**

Tool for automating CI/CD pipelines.

**7. CodePipeline**

AWS service for automating build, test, and deployment.

---

# 🚀 Conclusion

Successfully deployed a Docker-based web application and configured Nginx reverse proxy on an Amazon Linux EC2 instance with proper troubleshooting and automation script.

## 👨‍💻 Author

**Aman Lodha**  
Cloud Security

# 🧅 Onion Website using Raspberry Pi 5 + Docker + Tor

This project demonstrates how to host an anonymous `.onion` website using a Raspberry Pi 5, Docker, and Tor.

---

## 🚀 Features
- Anonymous hosting using Tor Hidden Services
- Docker-based setup (no need to install Tor on host)
- Lightweight and easy to deploy
- No public IP or domain required

---

## 🧰 Tech Stack
- Raspberry Pi 5 (16GB)
- Docker & Docker Compose
- Nginx (Web Server)
- Tor

---

## 📁 Project Structure
onion-site/
│── html/
│   └── index.html
│── tor/
│   ├── Dockerfile
│   └── torrc
│── Dockerfile
│── docker-compose.yml

---

## ⚙️ Setup Instructions

### 1. Install Docker
sudo apt update  
sudo apt install docker.io -y  

Enable Docker:  
sudo systemctl enable docker  
sudo systemctl start docker  

---

### 2. Install Docker Compose
sudo apt install docker-compose-plugin  

Check version:  
docker compose version  

---

### 3. Create Project
mkdir onion-site  
cd onion-site  
mkdir html tor  

---

### 4. Create Website
nano html/index.html  

Example:
<h1>🚀 My Onion Site</h1>  
<p>Running on Raspberry Pi with Docker & Tor</p>  

---

### 5. Create Web Dockerfile
nano Dockerfile  

Content:
FROM nginx:alpine  
COPY html /usr/share/nginx/html  

---

### 6. Configure Tor
nano tor/torrc  

Content:
HiddenServiceDir /var/lib/tor/hidden_service/  
HiddenServicePort 80 web:80  

---

### 7. Create Tor Dockerfile
nano tor/Dockerfile  

Content:
FROM alpine  
RUN apk add --no-cache tor  
COPY torrc /etc/tor/torrc  
CMD ["tor"]  

---

### 8. Docker Compose Setup
nano docker-compose.yml  

Content:
services:
  web:
    build: .
    container_name: onion_web
    restart: always

  tor:
    build: ./tor
    container_name: onion_tor
    depends_on:
      - web
    volumes:
      - tor_data:/var/lib/tor
    restart: always

volumes:
  tor_data:

---

### ▶️ Run the Project
docker compose up -d --build  

---

### 🔑 Get Onion URL
docker exec onion_tor cat /var/lib/tor/hidden_service/hostname  

---

## 🌐 Access the Site
Use Tor Browser and open:  
a5ja34glhjsg326gwbkpmqrxd2ipramlk7nsi4gpciyn4kualy3yf6yd.onion

---

## 🔄 Updating Content
Edit files inside the html/ directory and rebuild:  
docker compose up -d --build  

---

## 🔌 Behavior
- Internet OFF → site unreachable  
- Internet ON → site automatically available  

---

## 🔐 Key Learnings
- Docker containerization  
- Tor hidden services  
- Self-hosting infrastructure  

---

## 📌 Future Improvements
- Add backend (Node.js)  
- Authentication system  
- Admin dashboard  

---

## 👨‍💻 Author
Jaber Ahmed (Jack Sargey)

---

## ⭐ Support
If you like this project, give it a star ⭐ and share it!
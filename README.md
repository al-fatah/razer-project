# Docker-Nginx Load Balanced App with Private Registry (S3-Backed)

This project sets up a complete Dockerized deployment workflow using:

- ✅ A private Docker Registry with AWS S3 as backend storage
- ✅ Deployment of the `yeasy/simple-web` app from the private registry
- ✅ Nginx as a reverse proxy in front of a single app container
- ✅ Nginx as a load balancer for multiple instances of the app

---

## 📁 Project Structure

project/
├── docker-registry/
│   ├── docker-compose.yml
│   └── config.yml
├── nginx/
   ├── reverse-proxy/
   │   ├── default.conf
   │   └── docker-compose.yml
   └── load-balancer/
       ├── nginx.conf
       └── docker-compose.yml

---

## 🛠️ Step A: Set Up a Private Docker Registry (S3 Backend)

### 1. Create `.env` file in `docker-registry/`

Edit .env:

AWS_REGION=ap-southeast-1
AWS_BUCKET=my-docker-registry
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key

### 2. Start Registry

cd docker-registry
docker compose up -d

## 🚀 Step B: Deploy yeasy/simple-web via Private Registry

### 1. Pull, tag, and push image:

docker pull yeasy/simple-web
docker tag yeasy/simple-web localhost:5000/simple-web
docker push localhost:5000/simple-web

### 2. Run app using local image:

docker run -d -p 8080:80 localhost:5000/simple-web

## 🌐 Step C: Reverse Proxy with Nginx

### 1. Setup

cd nginx/reverse-proxy
docker compose up -d

Access the app at: http://<EC2-IP>:8081

## ⚖️ Step D: Load Balancing with Nginx

### 1. Setup

cd nginx/load-balancer
docker compose up -d

Access the load balancer at: http://<EC2-IP>:8082

Nginx will round-robin between simpleweb1 and simpleweb2.

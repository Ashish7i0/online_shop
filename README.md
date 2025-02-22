# 🍽️ Online Shop - Docker | Vite | React

Welcome to the **Online Shop** project! This guide provides a structured approach to setting up, running, and managing the project. 🚀

---

## 📋 Steps to Set Up and Run the Project

## 👇 FOR AWS USERS and LOCAL USERS

### 0⃣ Log in to AWS Console
- Log into the AWS Console and start an instance with **Ubuntu**.
- Save the key pair for secure access.

### 1⃣ Allow Port 3000
- Allow port **3000**, which will be used in this project:
  - Click on the instance and navigate to **Security Group**.
  - Edit **Inbound Rules**.
  - Add a rule for port **3000**.
  - Set it to **all IPv4** and save.

### 2⃣ Connect to the Instance
- Ensure the key is not publicly viewable:
  ```bash
  chmod 400 <downloaded-key.pem>
  ```
- Connect to the instance via **SSH**:
  ```bash
  ssh -i <downloaded-key.pem> ubuntu@<EC2-IP-Address>
  ```

### 3⃣ Update System
- Run the following command:
  ```bash
  sudo apt update
  ```

### 4⃣ Fork the Repository
- Fork the **Hackathon repository** to your GitHub account.

### 5⃣ Create a Project Directory
- Create a directory named `hackathon` and navigate into it:
  ```bash
  mkdir hackathon && cd hackathon
  ```

### 6⃣ Install Docker 🐋
- Install Docker using:
  ```bash
  sudo apt-get install docker.io -y
  ```

### 7⃣ Verify Docker Installation ✅
- Check if Docker is running:
  ```bash
  sudo systemctl status docker
  ```

### 8⃣ Add User to Docker Group 👤
- Add the current user to the Docker group:
  ```bash
  sudo usermod -aG docker $USER
  ```
- Refresh user groups:
  ```bash
  newgrp docker
  ```

### 9⃣ Clone the Repository 🛠️
- Generate a **Personal Access Token (PAT)** from **GitHub Settings > Developer Settings > PAT**.
- Clone the repository:
  ```bash
  git clone https://<username>:<PAT>@github.com/<username>/online_shop.git
  ```

### ♻️ Install Dependencies
- Navigate into the project directory:
  ```bash
  cd online_shop
  ```
- Install project dependencies:
  ```bash
  npm install
  ```

### 🔧 Modify the Code (Example: Changing UI Text)
- Open `src/App.jsx`:
  ```bash
  vim src/App.jsx
  ```
- Make necessary modifications, save changes, and exit.

### 🔄 Restart the App After Changes
- Stop the running instance:
  ```bash
  Ctrl + C
  ```
- Restart the application:
  ```bash
  npm run dev
  ```

---

## 🏢 Containerizing the Application with Docker

### 📝 Create a Multi-Stage Dockerfile
- Create and edit `Dockerfile`:
  ```bash
  vim Dockerfile
  ```
- Add the following content:
  ```dockerfile
  # Stage 1: Build Stage
  FROM node:18-alpine AS builder
  WORKDIR /app
  COPY package*.json ./
  RUN npm install
  COPY . .
  RUN npm run build

  # Stage 2: Production Stage
  FROM node:18-alpine
  WORKDIR /app
  COPY --from=builder /app /app
  EXPOSE 3000
  CMD ["npm", "run", "start"]
  ```

### 🔄 Build and Run Docker Container
- Build the Docker image:
  ```bash
  docker build -t online-shop .
  ```
- Run the container:
  ```bash
  docker run -p 3000:3000 online-shop
  ```

  ![image](https://github.com/user-attachments/assets/b05532be-2896-40a6-b836-69bc742ba06c)


---

## 🛠 Docker Compose Setup

### 📝 Create `docker-compose.yml`
- Create and edit `docker-compose.yml`:
  ```bash
  vim docker-compose.yml
  ```
- Add the following content:
  ```yaml
  version: "3.8"
  services:
    app:
      build:
        context: .
        dockerfile: Dockerfile
      ports:
        - "3000:3000"
      volumes:
        - .:/app
        - /app/node_modules
      environment:
        NODE_ENV: development
      command: npm run start
  ```

### 🛠 Run with Docker Compose
- Start the application:
  ```bash
  docker-compose up -d
  ```


  ![image](https://github.com/user-attachments/assets/8acd563e-798d-4d11-9541-cbc91329ab9e)

- Stop the application:
  ```bash
  docker-compose down
  ```

---

## 💒 Push Changes to GitHub

### 🌳 Create a New Branch
- Create a new branch:
  ```bash
  git checkout -b final-phase1
  ```

### 💌 Commit and Push Changes
- Add and commit changes:
  ```bash
  git add .
  git commit -m "Added Dockerfile and Docker Compose setup"
  git push origin final-phase1
  ```

---

## 🌟 Key Changes in `vite.config.js`
- Updated `vite.config.js` to ensure proper hosting and development environment setup:
  ```javascript
  import { defineConfig } from 'vite';
  import react from '@vitejs/plugin-react';

  export default defineConfig({
      plugins: [react()],
      base: './',
      css: {
          devSourcemap: false,
      },
      server: {
          port: 3000,
          host: true,
      },
  });
  ```

---

## 🎉 Congratulations!
- You have successfully set up and containerized the **Online Shop** project!



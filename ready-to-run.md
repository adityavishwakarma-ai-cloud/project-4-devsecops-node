## 1️⃣ `web/app.js` – Node.js Web Server

```javascript
// app.js
const express = require('express');
const app = express();
const port = 8080;

// Home route
app.get('/', (req, res) => {
  res.send('Hello! This is Project 4: DevSecOps Node.js App');
});

// Sample API route
app.get('/api/users', (req, res) => {
  const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' }
  ];
  res.json(users);
});

app.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

---

## 2️⃣ `web/package.json`

```json
{
  "name": "devsecops-node",
  "version": "1.0.0",
  "description": "Node.js app for DevSecOps Jenkins pipeline",
  "main": "app.js",
  "scripts": {
    "start": "node app.js",
    "test": "echo \"No tests configured\""
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 3️⃣ `docker-compose.yml`

```yaml
version: '3.8'

services:
  web:
    build: ./web
    ports:
      - "8080:8080"
    networks:
      - devsecops-network

networks:
  devsecops-network:
    driver: bridge
```

---

## 4️⃣ `.gitignore`

```gitignore
node_modules/
.env
*.log
backup/
```

---

## 5️⃣ `sonar-project.properties` – SonarQube Configuration

```properties
# Required metadata
sonar.projectKey=DevSecOpsNode
sonar.projectName=DevSecOps Node.js App
sonar.projectVersion=1.0

# Source files
sonar.sources=web

# Language
sonar.language=js

# Encoding
sonar.sourceEncoding=UTF-8
```

---

## 6️⃣ Folder Structure

```
project-4-devsecops-node/
│-- README.md
│-- Jenkinsfile
│-- docker-compose.yml
│-- web/
│   ├── app.js
│   └── package.json
│-- .gitignore
│-- sonar-project.properties
```

---

## ⚡ How to Test Locally

1. **Build Docker container**

```bash
docker-compose up -d --build
```

2. **Check the app in browser**

```
http://localhost:8080
```

3. **Run Jenkins pipeline**

* Configure Jenkins to clone this repo.
* Jenkinsfile runs build, test, SonarQube scan, Trivy scan, and deploys the app.

4. **Stop containers**

```bash
docker-compose down
```

---

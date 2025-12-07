# AWS Java Maven Jenkins CI/CD Pipeline

<div align="center">

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Maven](https://img.shields.io/badge/Maven-C71A36?style=for-the-badge&logo=apache-maven&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)
![GitLab](https://img.shields.io/badge/GitLab-FC6D26?style=for-the-badge&logo=gitlab&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)

</div>

## 📋 About

Demonstration project showcasing automated CI/CD pipeline for Java application using Jenkins Multibranch Pipeline, Docker, and AWS EC2. The project implements a complete workflow from automatic build on GitLab push to container deployment on AWS instance.

## 🎯 Key Features

- 🔄 **Automatic builds** via GitLab Webhook on every push
- 🚀 **Jenkins Multibranch Pipeline** for branch management
- 📦 **Automatic version incrementation** of the application
- 🐳 **Containerization** with Docker
- ☁️ **AWS EC2 deployment** via SSH
- 🔐 **Shared Library** for reusable Jenkins code
- 🗃️ **Multi-container setup** with PostgreSQL

## 🏗️ Architecture

```
GitLab (webhook) → Jenkins → Build JAR → Build Docker Image → Push to DockerHub → Deploy to AWS EC2
                                                                                         ↓
                                                                              Docker Compose (App + PostgreSQL)
```

## 🛠️ Technology Stack

### Backend
- **Java 8** - Programming language
- **Spring Boot 2.3.5** - Application framework
- **Maven 3.9** - Project build system
- **Amazon Corretto 8** - Java runtime in Docker

### CI/CD
- **Jenkins** - Build and deployment automation
- **Jenkins Shared Library** - Reusable pipeline functions
- **GitLab CI/CD** - Webhook triggers
- **Multibranch Pipeline** - Multi-branch management

### DevOps
- **Docker** - Application containerization
- **Docker Compose** - Container orchestration
- **DockerHub** - Image registry
- **AWS EC2** - Application hosting
- **SSH** - Remote server access

### Database
- **PostgreSQL 15** - Relational database

## 📂 Project Structure

```
.
├── src/                        # Application source code
├── Jenkinsfile                 # CI/CD pipeline configuration
├── Dockerfile                  # Docker image build instructions
├── docker-compose.yaml         # Multi-container setup configuration
├── server-cmds.sh             # EC2 deployment script
└── pom.xml                    # Maven configuration and dependencies
```

## 🔄 CI/CD Pipeline Stages

### 1️⃣ **Increment Version**
```groovy
- Automatic application version increment
- Using Maven build-helper plugin
- Generate IMAGE_NAME with version and BUILD_NUMBER
```

### 2️⃣ **Build App**
```groovy
- Build JAR file using Maven
- Using Shared Library buildJar() function
- Create Spring Boot executable JAR
```

### 3️⃣ **Build Image**
```groovy
- Build Docker image with new version
- Login to DockerHub
- Push image to registry
```

### 4️⃣ **Deploy**
```groovy
- Copy docker-compose.yaml and scripts to EC2
- SSH connection to AWS instance
- Start containers via docker-compose
```

### 5️⃣ **Commit Version Update**
```groovy
- Commit updated version in pom.xml
- Automatic push back to GitLab
- Preserve version history
```

## 🚀 Quick Start

### Prerequisites

- Java 8+
- Maven 3.6+
- Docker & Docker Compose
- Jenkins with installed plugins:
  - Pipeline
  - Git
  - SSH Agent
  - Credentials

### Jenkins Setup

1. **Create Multibranch Pipeline project**
2. **Configure GitLab webhook** for automatic triggers
3. **Add credentials:**
   - `github-credantilas` - for Shared Library access
   - `gitlab-pass` - for version changes push
   - `ec2-server-key` - SSH key for EC2 access
   - DockerHub credentials for image push

4. **Configure Maven tool** with name `maven-3.9`

### Local Build

```bash
# Clone repository
git clone https://github.com/DanilaNagornyi/aws-java-maven-jenkins.git
cd aws-java-maven-jenkins

# Build application
mvn clean package

# Build Docker image
docker build -t demo-app .

# Run with Docker Compose
export IMAGE=demo-app:latest
docker-compose up -d
```

Application will be available at: `http://localhost:8080`

## 🐳 Docker

### Dockerfile
```dockerfile
FROM amazoncorretto:8-alpine3.17-jre
EXPOSE 8080
COPY ./target/java-maven-app-*.jar /usr/app/
WORKDIR /usr/app
ENTRYPOINT ["java", "-jar", "java-maven-app-*.jar"]
```

### Docker Compose
```yaml
services:
  java-maven-app:
    image: ${IMAGE}
    ports:
      - 8080:8080
  postgres:
    image: postgres:15
    ports:
      - 5432:5432
    environment:
      - POSTGRES_PASSWORD=my-pwd
```

## ☁️ AWS Deployment

### EC2 Requirements
- Amazon Linux 2 or similar
- Docker and Docker Compose installed
- Port 8080 open in Security Group
- SSH access configured

### Deployment Script
```bash
#!/usr/bin/env bash
export IMAGE=$1
docker-compose -f docker-compose.yaml up --detach
echo "success"
```

## 📝 Versioning

Project uses automatic semantic versioning:
- **Major.Minor.Patch** format
- Automatic Patch version increment on each build
- BUILD_NUMBER appended to Docker image tag

Example: `1.1.5-42` where:
- `1.1.5` - version from pom.xml
- `42` - Jenkins build number

## 🔐 Security

- All credentials stored in Jenkins Credentials Manager
- SSH keys used for secure EC2 access
- Docker images contain only necessary runtime dependencies
- Alpine Linux used to minimize attack surface

## 📊 Monitoring

Application uses:
- **Logstash Logback Encoder** for structured logging
- **Spring Boot Actuator** endpoints for health checks
- Docker logs for container monitoring

## 🤝 Contributing

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is created for educational purposes.

## 👨‍💻 Author

**Danila Nagornyi**

- GitLab: [@DanilaNagornyi](https://gitlab.com/DanilaNagornyi)
- GitHub: [@DanilaNagornyi](https://github.com/DanilaNagornyi)

---

<div align="center">

⭐ Star this repo if you find it useful!

</div>

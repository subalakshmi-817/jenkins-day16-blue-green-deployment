# 🚀 Jenkins Day 16 – Blue-Green Deployment with Docker

## 📌 Project Overview

This project demonstrates a Blue-Green Deployment strategy using Jenkins, Docker, GitHub, and Linux.

Blue-Green Deployment is a deployment technique that maintains two separate environments:

- 🔵 Blue Environment – Current/active version
- 🟢 Green Environment – New version

The main purpose of Blue-Green Deployment is to reduce application downtime and deployment risk.

Instead of directly replacing the currently running application, the new version is deployed into a separate environment. After verifying that the new version is healthy, traffic can be switched from the old version to the new version.

This project implements the concept using Docker containers and Jenkins Pipeline automation.

---

# 📚 Table of Contents

1. Project Overview
2. Objectives
3. Problem Statement
4. Proposed Solution
5. What is Blue-Green Deployment?
6. Why Blue-Green Deployment?
7. Architecture
8. Technologies Used
9. Project Structure
10. Blue Environment
11. Green Environment
12. Docker Configuration
13. Jenkins Pipeline
14. Pipeline Stages
15. Health Checks
16. Deployment Workflow
17. Installation Requirements
18. Linux Setup
19. Docker Setup
20. Jenkins Setup
21. GitHub Integration
22. Project Execution
23. Manual Docker Deployment
24. Jenkins Automated Deployment
25. Verification
26. Rollback Strategy
27. Advantages
28. Limitations
29. Real-Time Use Cases
30. Security Considerations
31. Troubleshooting
32. Screenshots
33. Learning Outcomes
34. Future Enhancements
35. Conclusion
36. Author

---

# 🎯 1. Project Objectives

The main objectives of this project are:

- Understand Blue-Green Deployment.
- Learn how Docker containers can be used for separate environments.
- Automate deployment using Jenkins.
- Integrate GitHub with Jenkins.
- Build Docker images automatically.
- Deploy Blue and Green application versions.
- Perform application health checks.
- Verify running containers.
- Reduce deployment downtime.
- Understand CI/CD pipeline concepts.
- Learn practical DevOps workflow using Linux.

---

# ❗ 2. Problem Statement

Traditional deployment methods can cause application downtime.

For example, consider an application currently running in production.

When developers release a new version, the old application may need to be stopped before the new version is started.

The deployment process may look like:

Old Application
       ↓
Stop Application
       ↓
Remove Old Version
       ↓
Build New Version
       ↓
Deploy New Version
       ↓
Start Application

During this process, users may experience:

- Application downtime
- Errors
- Connection failures
- Incomplete deployment
- Unexpected bugs

A safer deployment strategy is required.

---

# 💡 3. Proposed Solution

This project uses Blue-Green Deployment.

Two environments are maintained:

Blue Environment
        ↓
Current Application

Green Environment
        ↓
New Application

The new version is first deployed to the Green environment.

After checking the Green environment:

- Container status is verified.
- Application response is checked.
- Health check is performed.
- Deployment is verified.

If everything is successful, Green becomes the new production version.

If the new version fails, traffic can remain on Blue.

---

# 🔵🟢 4. What is Blue-Green Deployment?

Blue-Green Deployment is a release management strategy where two identical production environments are maintained.

The environments are usually called:

- Blue
- Green

The Blue environment represents the currently active application.

The Green environment represents the new version.

Example:

        USERS
          |
          v
     +-----------+
     | Production|
     +-----------+
          |
          v
      🔵 BLUE
    Version 1.0

When Version 2.0 is ready:

      🟢 GREEN
    Version 2.0

The new version is deployed and tested in Green.

After successful verification:

        USERS
          |
          v
      🟢 GREEN
    Version 2.0

Blue can then remain available as a rollback environment.

---

# 🏗️ 5. Architecture

The architecture of this project is:

Developer
    |
    v
GitHub Repository
    |
    v
Jenkins
    |
    +-------------------+
    |                   |
    v                   v
Build Blue Image    Build Green Image
    |                   |
    v                   v
Docker Blue         Docker Green
Container           Container
    |                   |
    +---------+---------+
              |
              v
       Health Checking
              |
              v
       Deployment Verification

---

# 🛠️ 6. Technologies Used

## Operating System

Ubuntu Linux

## Version Control

Git

## Source Code Repository

GitHub

## CI/CD Tool

Jenkins

## Containerization

Docker

## Web Server

Nginx

## Programming / Markup

HTML

## Automation

Jenkins Declarative Pipeline

---

# 📁 7. Project Structure

The project contains the following structure:

jenkins-day16-blue-green-deployment/

├── blue/

│   ├── Dockerfile

│   └── index.html

│

├── green/

│   ├── Dockerfile

│   └── index.html

│

├── Jenkinsfile

│

├── screenshots/

│   ├── 01-project-folder.png

│   ├── 02-blue-green-files.png

│   ├── 03-blue-docker-build.png

│   ├── 04-blue-container-running.png

│   ├── 05-blue-application.png

│   ├── 06-blue-health-check.png

│   ├── 07-blue-green-images.png

│   ├── 08-both-containers-running.png

│   ├── 09-green-application.png

│   ├── 10-jenkins-docker-access.png

│   ├── 11-Jenkins-pipeline.png

│   ├── 12-new-job.png

│   └── 13-pipeline-scm-config.png

│

└── README.md

---

# 🔵 8. Blue Environment

The Blue environment represents the current application version.

The Blue Dockerfile uses Nginx as the web server.

Example:

FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

The Blue application is built as:

docker build -t day16-blue ./blue

The Blue container can be started using:

docker run -d \
--name day16-blue-container \
-p 8081:80 \
day16-blue

The application can then be accessed using:

http://localhost:8081

---

# 🟢 9. Green Environment

The Green environment represents the new application version.

Green uses its own Dockerfile and HTML page.

Build command:

docker build -t day16-green ./green

Run command:

docker run -d \
--name day16-green-container \
-p 8082:80 \
day16-green

The Green application can be accessed using:

http://localhost:8082

---

# 🐳 10. Docker Configuration

Docker provides isolated environments for the Blue and Green applications.

Docker allows us to:

- Create application images
- Run containers
- Stop containers
- Remove containers
- Manage application versions
- Isolate applications
- Reproduce deployment environments

Docker images:

day16-blue

day16-green

Docker containers:

day16-blue-container

day16-green-container

---

# ⚙️ 11. Jenkins Pipeline

Jenkins is used to automate the deployment process.

The Jenkinsfile contains multiple stages.

Typical stages include:

1. Checkout
2. Build Blue Image
3. Build Green Image
4. Remove Old Containers
5. Deploy Blue
6. Health Check Blue
7. Deploy Green
8. Health Check Green
9. Verify Deployment

---

# 🔄 12. Pipeline Workflow

The Jenkins workflow is:

GitHub
   |
   v
Jenkins Checkout
   |
   v
Build Blue Image
   |
   v
Build Green Image
   |
   v
Remove Old Containers
   |
   v
Deploy Blue
   |
   v
Blue Health Check
   |
   v
Deploy Green
   |
   v
Green Health Check
   |
   v
Verify Deployment
   |
   v
SUCCESS

---

# 🔨 13. Build Blue Image

Jenkins executes:

docker build -t day16-blue ./blue

This command:

- Reads the Blue Dockerfile.
- Uses Nginx as the base image.
- Copies index.html.
- Creates the Docker image.

---

# 🔨 14. Build Green Image

Jenkins executes:

docker build -t day16-green ./green

This creates the Green application image.

---

# 🧹 15. Remove Old Containers

Before deployment, old containers may need to be removed.

Example:

docker rm -f day16-blue-container || true

docker rm -f day16-green-container || true

The `|| true` prevents the pipeline from failing if a container does not already exist.

---

# 🚀 16. Deploy Blue

Blue can be deployed using:

docker run -d \
--name day16-blue-container \
-p 8081:80 \
day16-blue

This starts the Blue Nginx application.

---

# ❤️ 17. Blue Health Check

The application can be checked using:

curl http://localhost:8081

If the HTML page is returned successfully, the Blue environment is working.

Example:

curl -I http://localhost:8081

A successful response should contain:

HTTP/1.1 200 OK

---

# 🚀 18. Deploy Green

Green can be deployed using:

docker run -d \
--name day16-green-container \
-p 8082:80 \
day16-green

The Green environment runs separately from Blue.

---

# ❤️ 19. Green Health Check

The Green application can be checked using:

curl http://localhost:8082

Or:

curl -I http://localhost:8082

A successful response should return:

HTTP/1.1 200 OK

---

# 🔍 20. Deployment Verification

Docker containers can be checked using:

docker ps

Example:

CONTAINER ID
IMAGE
STATUS
PORTS
NAMES

The Blue and Green containers should show:

Up

The application can also be checked using:

docker logs day16-blue-container

docker logs day16-green-container

---

# 🖥️ 21. Manual Project Execution

Navigate to the project directory:

cd ~/Documents/jenkins-day16-blue-green-deployment

Build Blue:

docker build -t day16-blue ./blue

Build Green:

docker build -t day16-green ./green

Check images:

docker images

Run Blue:

docker run -d \
--name day16-blue-container \
-p 8081:80 \
day16-blue

Run Green:

docker run -d \
--name day16-green-container \
-p 8082:80 \
day16-green

Check containers:

docker ps

Test Blue:

curl http://localhost:8081

Test Green:

curl http://localhost:8082

---

# 🔗 22. GitHub Integration

The project is stored in GitHub.

Repository:

jenkins-day16-blue-green-deployment

Git is used for:

- Version control
- Source code management
- Collaboration
- Jenkins integration
- Tracking deployment changes

Basic commands:

git status

git add .

git commit -m "Update Day 16 deployment"

git push

---

# 🔧 23. Jenkins Configuration

Create a Jenkins Pipeline job.

Job name:

Day16-Blue-Green-Deployment

Select:

Pipeline

Then configure:

Definition:
Pipeline script from SCM

SCM:
Git

Repository URL:

https://github.com/subalakshmi-817/jenkins-day16-blue-green-deployment.git

Branch:

*/main

Script Path:

Jenkinsfile

Save the configuration.

---

# ▶️ 24. Running the Jenkins Pipeline

After configuring the Jenkins job:

1. Open the Jenkins job.
2. Click Build Now.
3. Open the build number.
4. Select Console Output.
5. Monitor the stages.
6. Verify Docker image creation.
7. Verify container deployment.
8. Verify health checks.

---

# 📊 25. Expected Pipeline Result

A successful pipeline should contain:

Checkout                    SUCCESS
Build Blue Image            SUCCESS
Build Green Image           SUCCESS
Remove Old Containers       SUCCESS
Deploy Blue                 SUCCESS
Health Check Blue           SUCCESS
Deploy Green                SUCCESS
Health Check Green          SUCCESS
Verify Deployment           SUCCESS

Final result:

SUCCESS

---

# 🔁 26. Rollback Strategy

One of the main benefits of Blue-Green Deployment is rollback.

Suppose:

Blue = Version 1.0

Green = Version 2.0

If Version 2.0 contains a serious problem:

Green ❌

The system can continue using:

Blue ✅

This reduces downtime and allows quick recovery.

---

# 🌍 27. Real-Time Example

Consider an online shopping application.

Current production:

Blue → Shopping Application v1.0

Developers release:

Green → Shopping Application v2.0

The new version is deployed to Green.

QA and automated health checks verify Green.

If Green works correctly:

Traffic → Green

If Green fails:

Traffic → Blue

This is useful for:

- E-commerce
- Banking applications
- SaaS platforms
- Web applications
- Enterprise systems
- Cloud applications

---

# ✅ 28. Advantages

## Zero or Minimal Downtime

Users can continue using the old environment while the new environment is being prepared.

## Fast Rollback

The old environment can remain available.

## Safer Deployment

The new version can be tested before becoming active.

## Easy Testing

Blue and Green can be tested independently.

## Automation

Jenkins automates the deployment process.

## Consistency

Docker provides consistent application environments.

---

# ⚠️ 29. Limitations

Blue-Green Deployment may require:

- Additional infrastructure
- More Docker resources
- More storage
- More monitoring
- Proper traffic switching
- Database migration planning

Running two complete environments can also increase resource consumption.

---

# 🔐 30. Security Considerations

Important security practices include:

- Protect Jenkins credentials.
- Use Jenkins credentials instead of hardcoding passwords.
- Use secure GitHub authentication.
- Restrict Docker permissions.
- Avoid exposing unnecessary ports.
- Keep Docker images updated.
- Keep Jenkins updated.
- Use HTTPS for production.
- Scan Docker images for vulnerabilities.

---

# 🐞 31. Troubleshooting

## Problem 1: Docker image build fails

Check:

docker --version

Check Docker service:

sudo systemctl status docker

Start Docker:

sudo systemctl start docker

---

## Problem 2: Docker permission denied

Check:

docker ps

If Jenkins cannot access Docker, verify Jenkins Docker group:

groups jenkins

If necessary:

sudo usermod -aG docker jenkins

Restart Jenkins:

sudo systemctl restart jenkins

---

## Problem 3: Port already in use

Check:

sudo ss -tulpn | grep 8081

or:

docker ps

Remove old container:

docker rm -f day16-blue-container

---

## Problem 4: Jenkins cannot find blue directory

Verify GitHub structure.

Correct:

blue/Dockerfile

green/Dockerfile

Incorrect:

jenkins-day16-blue-green-deployment/blue/Dockerfile

The Jenkins workspace must contain:

./blue

./green

---

## Problem 5: Jenkins cannot clone GitHub

Check:

- Repository URL
- Branch name
- Git installation
- Credentials
- Internet connection

---

## Problem 6: Health check fails

Check:

docker ps

Then:

docker logs day16-blue-container

or:

docker logs day16-green-container

Test manually:

curl http://localhost:8081

curl http://localhost:8082

---

# 📸 32. Screenshots

The screenshots directory documents the project implementation.

Example screenshots:

01-project-folder.png

Shows the Day 16 project directory.

02-blue-green-files.png

Shows Blue and Green application files.

03-blue-docker-build.png

Shows Docker image building.

04-blue-container-running.png

Shows Blue container running.

05-blue-application.png

Shows Blue application in browser.

06-blue-health-check.png

Shows Blue health check.

07-blue-green-images.png

Shows Docker Blue and Green images.

08-both-containers-running.png

Shows both containers running.

09-green-application.png

Shows Green application.

10-jenkins-docker-access.png

Shows Jenkins Docker configuration.

11-Jenkins-pipeline.png

Shows Jenkins Pipeline.

12-new-job.png

Shows Jenkins job configuration.

13-ppipeline-scm-config.png

Shows Pipeline SCM configuration.

---

# 🎓 33. Learning Outcomes

After completing this project, the following concepts were learned:

- Git and GitHub
- Jenkins
- Jenkins Pipeline
- Docker
- Dockerfile
- Docker images
- Docker containers
- Nginx
- CI/CD
- Blue-Green Deployment
- Health checks
- Deployment verification
- Rollback concepts
- Linux administration
- DevOps automation

---

# 🚀 34. Future Enhancements

This project can be improved by adding:

- Docker Compose
- Nginx reverse proxy
- Automatic traffic switching
- Load balancing
- Prometheus monitoring
- Grafana dashboards
- Automated rollback
- Docker image scanning
- Slack notifications
- Email notifications
- GitHub Webhooks
- Automated testing
- Kubernetes deployment
- AWS deployment
- HTTPS
- Production database migration strategy

---

# ☁️ 35. Cloud Deployment Possibility

The same concept can be deployed on cloud platforms.

Possible infrastructure:

GitHub
   |
   v
Jenkins
   |
   v
Docker
   |
   v
AWS EC2
   |
   +--------+
   |        |
 Blue      Green
   |        |
   +--------+
       |
       v
   Application

This provides a foundation for real-world DevOps deployment.

---

# 🧪 36. Testing Strategy

The deployment should be tested at multiple levels.

## Docker Image Test

docker images

## Container Test

docker ps

## Application Test

curl http://localhost:8081

curl http://localhost:8082

## Health Test

curl -I http://localhost:8081

curl -I http://localhost:8082

## Jenkins Test

Run the complete Jenkins pipeline.

## Deployment Test

Verify that both environments respond correctly.

---

# 📈 37. CI/CD Workflow

The complete DevOps workflow is:

Developer
    |
    v
Write Code
    |
    v
Git
    |
    v
GitHub
    |
    v
Jenkins
    |
    v
Checkout
    |
    v
Build Docker Images
    |
    v
Deploy Containers
    |
    v
Health Checks
    |
    v
Deployment Verification
    |
    v
Production

This demonstrates a practical CI/CD pipeline.

---

# 🏆 38. Project Achievement

This project demonstrates the practical implementation of:

GitHub → Jenkins → Docker → Blue-Green Deployment → Health Check

The project shows how DevOps tools can be integrated to automate application deployment.

---

# 📝 39. Conclusion

Jenkins Day 16 successfully demonstrates the Blue-Green Deployment strategy using Jenkins and Docker.

The project separates the application into two environments:

🔵 Blue

🟢 Green

The Jenkins pipeline automates the process of building Docker images, deploying containers, performing health checks, and verifying the deployment.

Blue-Green Deployment improves deployment safety because the new application version can be tested separately before becoming active.

The project also provides practical experience with:

- Linux
- Git
- GitHub
- Jenkins
- Docker
- CI/CD
- Nginx
- Deployment automation

This project provides a strong foundation for advanced DevOps concepts such as Kubernetes, cloud deployment, load balancing, monitoring, and automated rollback.

---

# 👩‍💻 Author

## Subalakshmi K

Computer Science Engineering Student

DevOps / Cloud / Linux Enthusiast

Project:

Jenkins Day 16 – Blue-Green Deployment with Docker

---

# ⭐ Final Workflow

GitHub
   ↓
Jenkins
   ↓
Checkout
   ↓
Build Blue
   ↓
Build Green
   ↓
Deploy
   ↓
Health Check
   ↓
Verify
   ↓
SUCCESS ✅

---

# 🚀 Jenkins + Docker + GitHub = Automated DevOps Deployment

This project demonstrates the practical use of CI/CD and Blue-Green Deployment in a Linux environment.

# 🧾 Project Documentation: Java Web Calculator Deployment using Two-Server Architecture

## 1. 📘 Project Overview

**Project Name:** Java Web Calculator  
**Objective:** Deploy a simple Java web application (WAR file) using a two-server setup:

- **Build Server:** For source code compilation, packaging, and artifact generation.  
- **Deploy Server:** For hosting and running the packaged WAR file using Apache Tomcat.

**Technology Stack:**  
- Java 17 (OpenJDK)  
- Apache Maven  
- Apache Tomcat 9.0.109  
- Git  
- Ubuntu 22.04  

---

## 2. 🖥️ Server Details

| Server Role | Hostname | Description |
|--------------|-----------|-------------|
| Build Server | java-cal-build | Used for building the Java application using Maven. |
| Deploy Server | java-cal-deploy | Used for deploying the .war file via Apache Tomcat. |

<img width="1591" height="350" alt="Image" src="https://github.com/user-attachments/assets/ace39e6d-3a18-406a-b152-d87bb6e732fb" />
---

## 3. 🧩 Project Flow

1. Clone the Java Web Calculator source code from GitHub on the build server.  
2. Install Java and Maven.  
3. Compile and package the application using Maven to generate a .war file.  
4. Securely copy (scp) the WAR file to the deploy server.  
5. Install Java and Apache Tomcat on the deploy server.  
6. Place the WAR file into Tomcat’s webapps directory.  
7. Start Tomcat and verify deployment.  

---

## 4. ⚙️ Build Server Configuration (`java-cal-build`)

### Step 1: Update and Set Hostname
```bash
sudo apt update
sudo hostnamectl set-hostname java-cal-build
sudo init 6
```

### Step 2: Install Git and Clone the Repository
```bash
git --version
git clone https://github.com/Ai-TechNov/JavaWebCalculator.git
cd JavaWebCalculator/
```
<img width="1295" height="668" alt="Image" src="https://github.com/user-attachments/assets/20f2bb29-8722-4f45-b5af-1d23ff12c80b" />

### Step 3: Install Required Packages
```bash
sudo apt install -y tree
sudo apt install openjdk-17-jre-headless -y
sudo apt install maven -y
```
<img width="874" height="467" alt="Image" src="https://github.com/user-attachments/assets/29a416d0-67ca-431c-b2bf-c28a6c90eeb7" />

### Step 4: Build and Package the Application
```bash
mvn validate
mvn package
```
<img width="1164" height="301" alt="Image" src="https://github.com/user-attachments/assets/4569020d-1003-4b89-9707-02d36757182a" />

---

<img width="944" height="423" alt="Image" src="https://github.com/user-attachments/assets/bd3c686f-0264-4a3d-9278-298797ace9d6" />

📦 **The .war file will be generated at:**  
`/home/ubuntu/JavaWebCalculator/target/webapp-0.2.war`

---

## 5. 📁 Post-Build Folder Structure

After successfully building the project, the final directory structure on the build server looks like this:

<img width="1651" height="936" alt="Image" src="https://github.com/user-attachments/assets/109b6b0f-37bf-4892-8ed3-6a218045664b" />

✅ **Key Points:**
- The `target/` folder contains all build outputs.  
- `webapp-0.2.war` is the deployable artifact.  
- Unit tests are executed, and reports are stored in `surefire-reports/`.  

---

## 6. 🚀 Deployment Server Configuration (`java-cal-deploy`)

### Step 1: Update and Set Hostname
```bash
sudo apt update
sudo hostnamectl set-hostname java-cal-deploy
sudo init 6
```

### Step 2: Install Java
```bash
sudo apt install openjdk-17-jre-headless -y
```

### Step 3: Install Apache Tomcat
```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.109/bin/apache-tomcat-9.0.109.tar.gz
tar -xvf apache-tomcat-9.0.109.tar.gz
```

### Step 4: Start Tomcat
```bash
cd apache-tomcat-9.0.109/bin
./startup.sh
```
<img width="942" height="240" alt="Image" src="https://github.com/user-attachments/assets/95d431f5-f0df-44d8-b515-61c72c9950c6" />
<img width="1599" height="718" alt="Image" src="https://github.com/user-attachments/assets/a1ff16aa-b95d-4092-8be2-071325e785db" />
### Step 5: Transfer WAR from Build Server
```bash
chmod 400 aja.pem
scp -i aja.pem /home/ubuntu/JavaWebCalculator/target/webapp-0.2.war ubuntu@54.91.238.45:/home/ubuntu/apache-tomcat-9.0.109/webapps
```

### Step 6: Verify Deployment

Once the WAR is placed, Tomcat automatically extracts it:  
`/home/ubuntu/apache-tomcat-9.0.109/webapps/webapp-0.2/`

Access via:  
`http://<Deploy-Server-IP>:8080/webapp-0.2`

---

## 7. 🧰 Troubleshooting

| Problem | Possible Cause | Solution |
|----------|----------------|-----------|
| Tomcat not starting | Java not installed | Install OpenJDK 17 |
| WAR not extracting | Wrong path or permissions | Verify webapps path and permissions |
| 404 Error | Wrong context path | Use correct URL `/webapp-0.2` |
| SCP fails | Invalid PEM key permissions | Run `chmod 400 aja.pem` |

---

## 8. 🧪 Testing

Check Tomcat:
```bash
sudo netstat -tulnp | grep 8080
```

Open the web calculator in your browser:  
`http://<Deploy-Server-IP>:8080/webapp-0.2`

Perform a few arithmetic operations to validate.
<img width="1919" height="735" alt="Image" src="https://github.com/user-attachments/assets/a81acb2a-aee8-481d-99b2-d283df9516ac" />

---

## 9. 🧱 Summary

| Component | Build Server | Deploy Server |
|------------|---------------|----------------|
| **OS** | Ubuntu 22.04 | Ubuntu 22.04 |
| **Java** | OpenJDK 17 | OpenJDK 17 |
| **Tool** | Maven | Tomcat 9.0.109 |
| **Main Role** | Compile and package | Host and run |

---

## 10. ✅ Outcome

Successfully built and deployed the **Java Web Calculator** using a two-server CI/CD-style workflow:

- Build server compiles and packages the WAR.  
- Deploy server hosts it on Tomcat.  

 <img width="1919" height="1026" alt="Image" src="https://github.com/user-attachments/assets/9a50dc9b-3bf9-4ed7-b21d-ca6cdad31e1e" />
---

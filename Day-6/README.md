# Day 6 - Jenkins Hands-on (03/04/2025)

## Jenkins Server Setup

- Instance Public IP: `http://107.21.155.134:8080`
- Retrieve Initial Admin Password:
  ```bash
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  ```
- Initial Setup Steps:
  - Install suggested plugins
  - Create first admin user:
    - Username: `athmika`
    - Password: 
    - Email: `athmikaubhat@gmail.com`
  - Save and continue until Jenkins dashboard loads

---

## Creating First Jenkins Job - JOB-01

- Navigate to: **New Item** → Enter `JOB-01` → Select *Freestyle project*
- In *Build Steps* → Add *Execute Shell*:
  ```bash
  echo "Hello Guys, "
  touch athmikait.txt
  echo "Hello Guys, Welcome to Jenkins Classes" >> athmikait.txt
  echo "Done..!!"
  ```
- Apply and Save → Click *Build Now*
- Verify:
  - Console output
  - Check file on server:
    ```bash
    cd /var/lib/jenkins/workspace/JOB-01
    cat athmikait.txt
    ```

---

## Shell Scripting Test

- Create and run a shell script manually:
  ```bash
  sudo su
  nano demo.sh
  ```
  Script contents:
  ```bash
  #!/bin/bash
  mkdir n1
  cd n1
  touch f1
  echo "Hello Guys" > f1
  ```
  Run the script:
  ```bash
  bash demo.sh
  ls n1
  cat n1/f1
  ```

---

## Jenkins Job with GitHub Repo + Maven (JOB-02)

- Install plugins:
  - Maven Integration Plugin
- Configure:
  - Manage Jenkins → Global Tool Configuration:
    - Enable automatic installation for Git and Maven
- Create a new job: `JOB-02` (Freestyle Project)
- Configure Source Code Management:
  - Git Repo: `https://github.com/ashokitschool/maven-web-app.git`
- Add build trigger:
  - Build periodically: `* * * * *` (every minute)
- Build step:
  - Invoke top-level Maven targets:
    - Goals: `clean package`
- Save and monitor builds

### Verify Workspace
```bash
cd /var/lib/jenkins/workspace/JOB-02
ls -l
cd target
ls
```

- Artifact: `maven-web-app.war`

---

## Jenkins + Git + Maven + Tomcat Deployment (JOB-03)

### Setup Tomcat Server (New EC2 Instance)

- Inbound Rules: Allow SSH, HTTP, HTTPS, and TCP 8080
- Install Java:
  ```bash
  sudo apt-get update
  sudo apt install openjdk-17-jdk default-jre fontconfig openjdk-17-jre
  ```

- Download and Extract Tomcat:
  ```bash
  wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.102/src/apache-tomcat-9.0.102-src.tar.gz
  tar -xvf apache-tomcat-9.0.102-src.tar.gz
  ```

- Allow remote access in `context.xml`:
  ```xml
  <Context antiResourceLocking="false" privileged="true" >
    <Valve className="org.apache.catalina.valves.RemoteAddrValve"
           allow=".*" />
  </Context>
  ```

- Configure `tomcat-users.xml`:
  ```xml
  <tomcat-users>
      <role rolename="admin-gui"/>
      <role rolename="manager-gui"/>
      <role rolename="manager-script"/>
      <role rolename="manager-jmx"/>
      <role rolename="manager-status"/>
      <user username="admin" password="admin" roles="admin-gui,manager-gui,manager-script,manager-jmx,manager-status"/>
  </tomcat-users>
  ```

- Make scripts executable and start Tomcat:
  ```bash
  cd bin/
  chmod +x catalina.sh startup.sh
  ./startup.sh
  ```

- Tomcat should be available at: `http://54.210.160.113:8080`

---

### Configure Jenkins Job - JOB-03

- Source Code Management:
  - Git Repo: `https://github.com/ashokitschool/JAVA-MAVEN-WEB-APP.git`
- Build Step:
  - Invoke Maven: `clean package`
- Post-build Action:
  - Deploy WAR/EAR to a container
  - WAR file path: `target/maven-web-app.war`
  - Container: Tomcat 9.x Remote
  - Tomcat URL: `http://54.210.160.113:8080/`
  - Credentials: `admin / admin`

---

## Notes

- If Tomcat fails to start:
  - Reboot EC2 instance
  - Navigate to `bin/` and run:
    ```bash
    ./shutdown.sh
    ./startup.sh
    ```

- Optional: Use precompiled Tomcat binaries instead of source version if build issues arise.

```

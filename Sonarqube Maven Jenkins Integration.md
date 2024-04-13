1. **Take 2 EC2 instances with ubuntu base and T2 medium size:**
   - One for Jenkins and Maven.
   - Another one for Sonarqube (Sonarqube won’t work with T2.micro).

Install Jenkins on Instance 1.

Install Sonarqube on instance 2.

**Jenkins installation:**
Follow the procedure outlined [here](https://github.com/siddeshpm1/Jenkins/blob/main/Jenkins%20installation.md).

**Sonarqube installation:**

- **Install Docker:**
  - Create an EC2 instance in an Amazon VPC.
  - **Updating package index:**
    ```
    sudo apt-get update
    ```
  - **Installing Docker:**
    ```
    sudo apt-get install docker.io -y
    ```
  - **Starting the Docker service:**
    ```
    sudo systemctl start docker
    ```
  - **Verifying the installation:**
    ```
    sudo docker run hello-world
    ```
  - **Enabling the Docker service:**
    ```
    sudo systemctl enable docker
    ```
  - **Check the Docker version:**
    ```
    docker --version
    ```
  - **Add User to Docker Group:**
    ```
    sudo usermod -a -G docker $(whoami)
    newgrp docker
    ```
- **Run the Sonarqube image to create the Sonarqube server:**
  - **Running SonarQube on Docker:**
    ```
    docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
    ```
  - **Login to Sonarqube:**
    - Username: admin
    - Password: admin
  - **Change the username and password after logging in.**
  - **Generate Token:**
    - Go to Administration > Security > Users > Administrator user > Click on 3 dots of tokens > Update token > Generate token.
    - Enter token name, expiry date, and generate. Keep the token safe to integrate with Jenkins.
  - **Configure Webhooks:**
    - Go to Administration > Configuration > Webhooks > Create new webhook.
    - Name: Jenkins webhook
    - URL: Jenkins URL: http://44.201.131.148:8080/sonarqube-webhook/ (/sonarqube-webhook/ is important, without this, the webhook won’t work).

- **Start the code analysis:**
  - Login to Jenkins.
  - **Install the required plugins:**
    - Docker Plugin
    - SonarQube Scanner for Jenkins.
  - **Install Maven on the Jenkins server:**
    ```
    sudo apt-get update
    sudo apt-get install maven
    ```
  - Go to Manage Jenkins > Systems > SonarQube servers.
    - Name: sonarserver
    - Server URL: http://44.203.131.83:9000/

- **Create a pipeline job:**
  - Select GitHub hook trigger for GITSCM polling.
  - Select pipeline.
  - **Repository URL:**
    ```
    https://github.com/nimbuswiztech/hello-world-with-maven.git
    ```
  - **Branch:**
    ```
    */main
    ```
  - **Script path:**
    ```
    Jenkinsfile
    ```
  - **Execute the Job**

  - **Check the Sonarqube Dashboard for the Analysis after the pipeline Execution**

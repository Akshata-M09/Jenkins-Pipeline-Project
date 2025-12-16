
# Jenkins CI/CD Pipeline: Spring Petclinic
This repository contains a full Jenkins CI/CD pipeline setup to build, test, analyze, and deploy the Spring Petclinic Framework app using SonarQube, Nexus Repository, and Tomcat.



## Pipeline Overview
The pipeline performs these stages:

- Checkout source code from GitHub
- Build and compile with Maven
- Run unit tests and collect reports
- Perform SonarQube static code analysis & quality gate check
- Upload WAR artifact to Nexus Maven repository
- Deploy WAR to Tomcat server
## Prerequisites
- Setup all four servers before starting the integration process
- For Jenkins server,install java-17,maven and Jenkins.Enable port 8080 and configure Jenkins.

      sudo apt update
      sudo apt install openjdk-17-jdk -y
      sudo apt install maven -y
- For SonarQube,install java,SonarQube through link,start SonarQube and enable port 9000.Sign in and configure SonarQube.

- For Nexus,install java,Nexus through link,start Nexus and enable port 8081.Sign in and configure Nexus.

- For Tomcat,install java,Tomcat through link,start Tomcat and enable port 8080.Sign in and configure Tomcat.

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/tomcat/Screenshot%20(979).png)
## SonarQube setup

 #### Generate a SonarQube Token

- Log in to SonarQube as an administrator.

- Navigate to Administration → Security.

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/sq/Screenshot%20(960).png)

- Create a new token by providing a name.

- Click Generate and copy the token securely. 



#### Add the Token to Jenkins Credentials

- Open Jenkins Dashboard → Manage Jenkins → Credentials.

- Click on System (Global).

- Select Add Credentials.

- Choose Secret Text as the credential type.

- Paste the generated SonarQube token into the Secret field.

- Enter the token name as the ID.

- Add a short description and click Add Credentials.

#### Install SonarQube Scanner Plugin

- In Jenkins, go to Manage Jenkins → Manage Plugins.

- Install the plugin named SonarQube Scanner.

#### Configure SonarQube Server in Jenkins

- Navigate to Manage Jenkins → Configure System.

- Scroll to the SonarQube Scanner section.

- Click Add SonarQube to set environment variables.

- Provide a name for the server (e.g., sonarqube).

- Enter the SonarQube Server URL.

- Under Credentials, select the token created earlier.

- Click Apply and then Save.

####  Add SonarQube Stage to Jenkins Pipeline

- Open your CI/CD pipeline job in Jenkins.

- Go to Configure and add a new pipeline stage for SonarQube analysis.

- In Pipeline Syntax, search for the template withSonarQubeEnv under sample steps.

#### Update Pipeline with Generated Syntax

- Copy the generated pipeline syntax.

- Paste it into the newly created SonarQube stage.

- Update the credentialsId and installationName if required.

- Run the pipeline to generate the SonarQube analysis report.

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/sq/Screenshot%20(961).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/sq/Screenshot%20(982).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/sq/Screenshot%20(981).png)

## Nexus setup

 #### Go to Manage Jenkins → Manage Plugins

- Install the plugin “Nexus Artifact Uploader”
  
  ![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/a39f81f59cabdcf7c02ca7454d47d5278cac8c35/ss/mexus/Screenshot%20(962).png)
 
#### Navigate to Manage Jenkins → Credentials



#### Add new credentials for Nexus:

- Select Username with password as the credential type

- Enter the Nexus username and password
- Provide a short description
- Click Create

#### Open your CI/CD pipeline job in Jenkins

- Click Configure

- Add a new stage for Nexus

#### In Pipeline Syntax:

- Select Nexus Artifact Uploader as the Sample Step

#### Fill in the required details:

- version

- artifactId

- groupId

   (Use values from the pom.xml file of the GitHub repository, e.g., spring-petclinic)

#### In the Nexus Version field:
- Select nexus3

#### In the Nexus URL field:
- Enter the public IP address followed by port 8081
#### Under the Repository section:

- Go to Nexus → Repositories → spring-petclinic

- Copy only the repository name (not the full URL)

- Paste it into the repository field

#### Click Generate Script

#### Copy the generated script and paste it into the Nexus stage of your Jenkins pipeline

     stage('nexus') {
      steps {
        nexusArtifactUploader artifacts: [[
          artifactId: 'spring-framework-petclinic',
          classifier: '',
          file: 'target/petclinic.war',
          type: 'war'
     ]],
        credentialsId: 'nexus',
        groupId: 'org.springframework.samples',
        nexusUrl: '144.57.35.72:8081',
        nexusVersion: 'nexus3',
        protocol: 'http',
        repository: 'spring-petclinic',
        version: '6.2.8'
      }
    }

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(963).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(964).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(965).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(966).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(967).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(968).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(969).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(970).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(971).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(972).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(973).png)

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/8cca3060544b854bc326169f64d9d9bc13c8b673/ss/mexus/Screenshot%20(974).png)




## Tomcat setup

- Go to Jenkins Dashboard and install the plugin named ssh-agent.

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/3cc35c3d29009a50e1b26323c5fe83a9607ffe4c/ss/tomcat/Screenshot%20(975).png)

- Navigate to Manage Jenkins → Credentials.

 - Add new credentials specifically for Tomcat.

- In the Kind dropdown, select SSH Username with private key.

- Enter:

    - ID and Description

    - Username: ubuntu

- Paste the private key content and click Create to save the credentials.

- Open your CI/CD pipeline job and add a new stage for Tomcat deployment.

- In Pipeline Syntax, select ssh-agent as the sample step.

- Ensure the credentials are selected automatically, then copy the generated syntax.

- Paste the syntax inside the Tomcat deployment stage.

![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/3cc35c3d29009a50e1b26323c5fe83a9607ffe4c/ss/tomcat/Screenshot%20(983).png)

- Add an scp command to copy the petclinic.war file from Jenkins to the Tomcat server.

Example 

      Jenkins Pipeline Stage

    stage('tomcat') {
    steps {
        sshagent(['tomcat']) {
            sh '''
            scp -o StrictHostKeyChecking=no target/petclinic.war \
            ubuntu@19.123.45.199:/home/ubuntu/apache-tomcat-10.1.43/webapps
            '''
        }
    }
    }

- Final Steps

   - Run the Jenkins build.

  - Click Save, then Build Now and ensure the build completes successfully.

  ![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/3cc35c3d29009a50e1b26323c5fe83a9607ffe4c/ss/tomcat/Screenshot%20(976).png)

  -  Access the application:

         http://<tomcat_public_ip>:8080/petclinic

  ![image alt](https://github.com/Akshata-M09/Jenkins-Pipeline-Project/blob/3cc35c3d29009a50e1b26323c5fe83a9607ffe4c/ss/tomcat/Screenshot%20(978).png)       

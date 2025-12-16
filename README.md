

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



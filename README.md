## Complete-the-CI-CD-Pipeline--Docker-Compose-Dynamic-versioning

### Technologies used:

AWS, Jenkins, Docker, Linux, Git, Java, Maven, Docker Hub

### Project Description:

CI step: Increment version

CI step: Build artifact for Java Maven application

CI step: Build and push Docker image to Docker Hub CD step: Deploy new application version with Docker Compose

CD step: Commit the version update

Here are the step-by-step instructions for completing the CI/CD pipeline for the Demo Project using Docker-Compose and dynamic versioning:

### 1. Set up an AWS EC2 instance and install Docker, Git, and Jenkins on it.

### 2. Create a new Jenkins pipeline project and configure it to retrieve the source code from the Git repository where the Java Maven application is stored.

### 3. Add a "Version Increment" step to the Jenkins pipeline. You can use the Maven Versions Plugin to increment the version number in the POM.xml file automatically. Here's an example command to increment the version by one minor version:

  mvn versions:set -DnewVersion=\${parsedVersion.nextMinorVersion}

### 4. Add a "Build Artifact" step to the Jenkins pipeline. This step will compile the Java code, package it into a JAR file, and store the JAR file in the "target" directory. Here's an example command:

  mvn clean package

### 5. Add a "Docker Build and Push" step to the Jenkins pipeline. This step will create a Docker image from the JAR file and push it to Docker Hub. Here's an example command:

    docker build -t your-dockerhub-username/demo-project:\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion} .
  docker push your-dockerhub-username/demo-project:\${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}

### 6. Create a Docker Compose file that defines the service for the Java Maven application. Here's an example Docker Compose file:

  version: '3'
services:
  demo-project:
    image: your-dockerhub-username/demo-project:\${DEMO_PROJECT_VERSION}
    ports:
      - "8080:8080"

Note the use of the "${DEMO_PROJECT_VERSION}" variable, which will be replaced with the new version number during the CD step.

### 7. Add a "Deploy with Docker Compose" step to the Jenkins pipeline. This step will deploy the new version of the Java Maven application using Docker Compose. Here's an example command:

    export DEMO_PROJECT_VERSION=\$(cat pom.xml | grep -m1 '<version>' | sed -E 's/[^0-9]*([0-9]+\.[0-9]+\.[0-9]+).*/\1/')
  docker-compose -f docker-compose.yml up -d
  
Note the use of the "export" command to set the "${DEMO_PROJECT_VERSION}" variable, which is used in the Docker Compose file.

### 8. Finally, add a "Commit Version Update" step to the Jenkins pipeline. This step will commit the version update to the Git repository. Here's an example command:

    git add pom.xml
  git commit -m "Version \${parsedVersion.majorVersion}.\${parsedVersion.minorVersion}.\${parsedVersion.incrementalVersion}"
  git push
  
With these steps, your Demo Project will have a complete CI/CD pipeline that uses Docker-Compose and dynamic versioning.




  

    
     






























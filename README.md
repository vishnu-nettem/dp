Program 1 and 2:
Go to git bash page and download win set up
Go to github and login – 
Create a github repo
git config --global user.name "name"
git config --global user.email "mail@gmail.com"
Open git bash
cd path/folder
git clone <repo-url>
cd <repo-folder>
touch test.txt
git add .
git commit -m “Initial commit”
git push
git branch b1
git checkout b1
touch test2.txt
git add .
git commit -m “branch commit”
git push
Go to pull requests in Github -> compare and pull request -> create a pull request -> merge pull request

**************************************************************************************************************************************
**************************************************************************************************************************************

Program 3:
Login to Github and create a repo
Click on Add file -> create new file
---------------------------------------------------------------------------
.github/workflows/action.yml   ********************
name: My First GitHub Actions

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        python-version: [3.8, 3.9]
 
    steps: 

    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v2
      with:
        python-version: ${{ matrix.python-version }}

    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install pytest

    - name: Run tests
      run: |
        cd src
        python -m pytest addition.py	
---------------------------------------------------------------------------
Commit changes
Add file -> create new file
---------------------------------------------------------------------------
src/addition.py   ******************************
def add(a,b):
  return a+b

def test_add():
  assert add(1,2) == 3
  assert add(1,-1) == 0
---------------------------------------------------------------------------
commit changes

Show the build details

mkdir actions-runner
cd actions-runner
curl -L -o <file-name.zip> <url>

use config commands


**************************************************************************************************************************************
**************************************************************************************************************************************

Program 4 and 5:
Visit https://github.com/docker-archive/toolbox/releases and download toolbox
or download docker Desktop from https://docs.docker.com/desktop/setup/install/windows-install/ 
Checks:
Wsl installed – if not wsl --install
Compatible windows version
Turn on wsl-2 in windows
Enable virtualization
If required also install virtual box

-Complete the install for toolbox or docker desktop on default setting
Commads (basic commands program 4)
docker --version
docker images
docker pull redis
docker images
docker ps
docker run redis     or    docker run --privileged redis
docker ps
docker exec -it <contain-id> sh
docker stop <contain-id>
docker ps
docker ps -all
docker rm <contain-id>
docker rmi redis
To build and image of webpage (program 5)
Open a sampleWebApp folder in vscode
---------------------------------------------------------------------------
add index.html ********************
<!DOCTYPE html>
<html>
<head>
    <title>Sample Web App</title>
</head>
<body>
    <h1>Welcome to the Sample Web Application!</h1>
    <p>This is a basic HTML and JavaScript application running in a Docker container.</p>
    <script>
        console.log('Hello from JavaScript!');
    </script>
</body>
</html>
---------------------------------------------------------------------------
Add Dockerfile **********************************
# Use an official Nginx image as the base image
FROM nginx:alpine

# Copy the HTML file to the Nginx default directory
COPY index.html /usr/share/nginx/html/

# Expose port 80
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
---------------------------------------------------------------------------
Open docker terminal
cd sampleWebApp folder
docker build -t sample-web-app .
docker images
docker run -d -p 8080:80 sample-web-app
docker ps
docker-machine ip
Open browser and search <ip>:8080
docker stop <contain-id>

**************************************************************************************************************************************
**************************************************************************************************************************************

Program 6:
Java Installation:
Install java 17 using wind-msi in oracle
Environment varibles -> add JAVA_HOME (jdk-17 folder path) and path (jdk-17/bin path)

Jenkins installation:
visit jenkins.io -> download -> under LTS -> click windows
Note: During installation -> select run service as LocalSystem
Give path for JDK (17 or 21) folder
Complete Installation
Open browser -> localhost:8080 -> provide adminitrator password -> install sugeested plugins -> create username and password

SonarQube installation:
visit SonarQube.com -> download sonarqube community edition 
Extract the zip file in C drive (c:\SonarQube\)
Open cmd -> cd StartSonar.bat file -> StartSonar.bat ->localhost:9000
Default id pass are admin -> change the pass

Procedure:
Open SonarQube -> Create a project -> Give a name -> use global settings -> create project
Select locally -> give a token name and generate -> copy the token

Open Jenkins -> Manage jenkins -> credentials -> add credentails
Select secret text and paste the token in secret section -> give token id and desc

Manage Jenkins -> Plugins -> install SonarQube Scanner for Jenkins, Pipeline, Git

Manage Jenkins -> System -> SonarQube Servers
Check env variables
Name: Sonarqube
server url: http://<ip-address>:9000
token: select new token

New Item -> name and freestyle-project -> ok
Source code management -> git -> repo (https://github.com/sunny-shaw/retail-store.git) -> branch (main)
Build envirnment -> Prepare SonarQube Scanner env -> select the token
Build Steps -> execute SonarQube scanner ->
Analysis properties:
sonar.projectKey=<Projectkey>
sonar.projectName=<Projectname>
sonar.projectVersion=1.0
sonar.language=java
sonar.tests=src/test/java
sonar.sources=src/main/java

Save -> build now -> console output -> report link

**************************************************************************************************************************************
**************************************************************************************************************************************

Program 7:
Maven Installation:
Download maven from https://maven.apache.org/download.cgi by clicking on Binary zip archive
Extract the zip file in Program files of local disk
In env varibles: ac to folder
MAVEN_HOME: C:\Program Files\apache-maven-3.9.9
path: C:\Program Files\apache-maven-3.9.9\bin
check maven installation : mvn --version in cmd

Jenkins Installation:
visit jenkins.io -> download -> under LTS -> click windows
Note: During installation -> select run service as LocalSystem
Give path for JDK (17) folder
Complete Installation
Open browser -> localhost:8080 -> provide administrator password -> install sugeested plugins -> create username and password

Manage Jenkins -> Plugins -> Pipeline, Git, Maven Integration plugin

Managen Jenkins -> tools -> 
JDK installations -> name: JDK_17 , JAVA_HOME: C:\Program Files\Java\jdk-17
GIT installation -> name: Git , Path to Git executable: C:\Program Files\Git\bin\git.exe
Maven installation -> name:  maven-local , MAVEN_HOME: C:\Program Files\apache-maven-3.9.9

Create New Item -> name and pipeline
Under Pipeline Script ********************************
pipeline {
    agent any
    
    environment{
    JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
    }
    
    tools{ 
        maven 'maven-local'
    }

    stages {
        stage('Git') {
            steps {
                git url:'https://github.com/ravdy/hello-world.git'    
            }
        }
        stage('build') {
            steps {
                bat "mvn clean install"
            }
        }
        stage('Test') {
            steps {
                bat 'mvn test'
            }
        }
    }
}
-------------------------------------------------------------
save and build

or (https://github.com/Sanchitk06/hello-world.git) for jdk 21

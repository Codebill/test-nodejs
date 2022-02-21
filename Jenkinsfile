pipeline{
  agent any
  tools{
    maven "maven3.8.4"
  }
  triggers{
    pollSCM('* * * * *')
  } 
  options{
    timestamps()
    buildDiscarder(logRotator(artifactDaysToKeepStr: '30', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '2'))
  }
 
  stages{
    stage('GitClone'){
      steps{
        sh "echo cloning code from github"
        git " https://github.com/Codebill/spring-boot-docker.git"
    }
  }
    stage('BuildandTest'){
      steps{
        sh "echo build and test packages"
        sh "mvn clean package"
    }
  }
    stage('CodeQuality'){
      steps{
        sh "echo code quality report"
        //sh "mvn sonar:sonar"
    }
  }
    stage('UploadtoNexus'){
      steps{
        sh "echo uploading artifacts to nexus"
        //sh "mvn deploy"
    }
  }
    stage('dockerlogin'){
      steps{
        withCredentials([string(credentialsId: 'dockerhubcredentials', variable: 'dockerhubcredentials')]) {sh "docker login -u codebillion -p ${dockerhubcredentials}"
        // some block
    }
  }
    stage('ImageBuild'){
      steps{
        sh "docker build -t codebillion/spring-boot-docker:5 ."
        sh "docker push codebillion/spring-boot-docker:5"
    }
  }
    stage('deployment'){
      steps{
        sh "docker rm -f myapp"
        sh "docker run --name myapp -d -p 7000:8080 codebillion/spring-boot-docker:5"
    }
  }
 
}
 
}


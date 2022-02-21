node{
    def mavenHome = tool name: 'maven3.8.4', type: 'maven'
    stage('SCM clone')
    {
    git credentialsId: 'Githulb_credential', url: 'https://github.com/Codebill/spring-boot-docker.git'
    }    
    stage('mvn build')
    { 
    sh "${mavenHome}/bin/mvn clean package" 
    }
    stage('3.CodeQualityReport')
    {
    //sh "${mavenHome}/bin/mvn sonar:sonar"
    }
    stage('4.UploadWarNexus')
    {
    //sh "${mavenHome}/bin/mvn clean deploy"
    }
    stage('5.DeployTomcat')
    {
    //deploy adapters: [tomcat9(credentialsId: 'tomcat-credential', path: '', url: 'http://23.239.31.97:8080/')], contextPath: null, war: 'target/*.war'
    }   
    stage('ImageBuild')
    {
    sh "docker build -t codebillion/spring-boot-docker:4 ."
    }
    stage('dockerlogin')
    {
    withCredentials([string(credentialsId: 'dockerhubcredentials', variable: 'dockerhubcredentials')]) {sh "docker login -u codebillion -p ${dockerhubcredentials}"
    // some block
    }
    } 
    stage('dockerpush')
    { 
    sh "docker push codebillion/spring-boot-docker:4"         
    }
    stage('deployment')
    {
    sh "docker rm -f myapp"
    //sh "docker run --name myapp -d -p 7000:8080 codebillion/spring-boot-docker:4"
    }
    stage('removedockerimages')
    { 
    sh 'docker rmi $(docker images -q)'        
    }
    stage('deploytok8scluster')
    { 
    sh 'kubectl apply -f springapp.yml'        
    }
}   


  
    
    
    
    


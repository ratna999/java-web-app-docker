node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/ratna999/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven3", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t 14551a0586/java-web-app .'
    }
    
    stage('Push Docker Image'){
        stage('Docker Image Push'){
   withCredentials([usernamePassword(credentialsId: 'Docker_Hub_Pwd', passwordVariable: 'ratna_pwd', usernameVariable: 'ratna_user')]) {
    sh "docker login -u ${ratna_user} -p ${ratna_pwd}"
   }
        }
        sh 'docker push 14551a0586/java-web-app'
     }
     
      stage('Run Docker Image In Dev Server'){
        
        def dockerRun = ' docker run  -d -p 8080:8080 --name java-web-app 14551a0586/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.6.209 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.6.209 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.6.209 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.6.209 ${dockerRun}"
       }
       
    }
     
     
}

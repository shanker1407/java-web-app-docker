node{
    
    stage("Git Clone"){
        
        git url: 'https://github.com/shanker1407/java-web-app-docker.git', branch: 'master'
    }
    
    stage(" Maven Clean Package"){
        def mavenHome =  tool name: "Maven", type: "maven"
        sh "${mavenHome}/bin/mvn clean package"
        
    }
    
    stage('Build Docker Image'){
        sh 'docker build -t udaykarthick/java-web-app .'
    
    }
    
    stage("Docker Login And Push"){
        withCredentials([string(credentialsId: 'DockerHubPwd', variable: 'DockerHubPwd')]) {
            sh "docker login -u udaykarthick -p ${DockerHubPwd}"
        }
        sh 'docker push udaykarthick/java-web-app:latest'
    }
    stage('Run Docker Image In Devployment Server'){
        
        sshagent(['Docker_Dev_Server_SSH']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.5.111 docker rm -f java-web-app || true'
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.5.111 docker run  -d -p 8080:8080 --name java-web-app udaykarthick/java-web-app:latest'  
        }
    }
}

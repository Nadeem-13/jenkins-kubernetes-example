pipeline{
    agent any
    stages {
        stage('Build Maven') {
            steps{
                 git credentialsId: 'GIT_CREDENTIALS', url: 'https://github.com/Nadeem-13/jenkins-kubernetes-example.git'
             
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t 007786/nodejsapp-1.0:latest .'
                }
            }
        }
        stage('Deploy Docker Image') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'Docker_HUB_PASSWORD', variable: 'PASSWORD')]) {
                    sh 'docker login -u 007786 -p $PASSWORD'
                 }  
                 sh 'docker push 007786/nodejsapp-1.0:latest'
                }
            }
        }
    
    stage('Deploy App on k8s') {
      steps {
            sshagent(['k8s']) {
            sh "scp -o StrictHostKeyChecking=no nodejsapp.yaml ubuntu@3.114.59.0 :/home/ubuntu"
            script {
                try{
                    sh "ssh ubuntu@3.114.59.0  kubectl create -f ."
                }catch(error){
                    sh "ssh ubuntu@3.114.59.0  kubectl create -f ."
            }
}
        }
      
    }
    }
    }
}


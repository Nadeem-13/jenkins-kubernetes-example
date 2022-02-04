pipeline{
    agent any
    environment {

        ProjectName = "nadeem123"
    
        registry = "494724829840.dkr.ecr.ap-northeast-1.amazonaws.com/demo-project"
	}

    stages {
        stage('Build Maven') {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'Nadeem-13', url: 'https://github.com/Nadeem-13/jenkins-kubernetes-example.git']]])

            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                  sh 'docker build -t 007786/nodejsapp-1.0:$BUILD_NUMBER .'
                  
                }
            }
        }
         stage('Building image') {
        steps {
         script {
           dockerImage = docker.build registry
        }
      }
    }
     stage('Pushing to ECR') {
     steps {  
         script {
                sh 'aws ecr get-login-password --region ap-northeast-1 | docker login --username AWS --password-stdin 494724829840.dkr.ecr.ap-northeast-1.amazonaws.com'
                
                sh 'docker push 494724829840.dkr.ecr.ap-northeast-1.amazonaws.com/demo-project:latest'
         }
        }
      }
    
    

stage("EKS") {
           steps{
                withCredentials([[
                $class: 'AmazonWebServicesCredentialsBinding',
                credentialsId: "aws-key",
                accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                 secretKeyVariable: 'AWS_SECRET_ACCESS_KEY'
             ]]) {
               
               sh ' aws eks --region ap-northeast-1 update-kubeconfig --name new-one-cluster'
               sh 'kubectl apply -f nodejsapp.yaml'
               
             }
           }
      }
}
}

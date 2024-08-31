pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                git 'https://github.com/ayyappanprofile2021/java-azure-project.git'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Build Docker OWN image') {
            steps {
                sh "sudo docker build -t ayyappanprofile/javaapp:latest ."

            }
        }
        
        stage('docker push ') {

            steps {
                withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')])  {
                // some block
                sh "sudo docker login -u ayyappanprofile -p $DOCKER_HUB_PASS_CODE"
                }
            sh "sudo docker push ayyappanprofile/javaapp:latest"
            }
        } 
        
        stage('Deploy webAPP in Prod Env') {
            steps {
                sshagent(['e99300eb-cb0b-4f11-acea-a57fcbe5ee77']) {
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@54.169.119.225   sudo wget https://raw.githubusercontent.com/ayyappanprofile2021/java-azure-project/master/k8-dep-svc.yml"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@54.169.119.225   sudo kubectl apply -f k8-dep-svc.yml"
                    }
            }
        }
            
        
        
        
    }
}

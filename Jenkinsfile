pipeline
{
    agent any
    
    stages
    {
        stage('SCM')
         {
             steps{
                 git 'https://github.com/ayyappanprofile2021/java-azure-project.git'
             }
         }
         stage('Package')
         {
             steps{
                 sh 'mvn clean package'
             }
         }
         stage('Build Docker Image')
         {
            steps{
                sh 'sudo docker build -t ayyappanprofile/javaapplication:latest .' 
            }
         }
         stage('Docker Image to Hub')
         {
             steps
             {
                 withCredentials([string(credentialsId: 'DOCKER_HUB_PWD', variable: 'DOCKER_HUB_PASS_CODE')]) {
                         // some block
                  sh 'sudo docker login -u ayyappanprofile -p $DOCKER_HUB_PASS_CODE'
                }
                sh 'sudo docker push ayyappanprofile/javaapplication:latest'
             }
         }
         
         stage('Deploy Docker in Prod using Kubernetes.')
         {
             steps
             {
                 sshagent(['e04e32f9-422e-4c0e-b222-0e24c04f87d6']) {
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@47.129.207.94 sudo wget https://raw.githubusercontent.com/ayyappanprofile2021/java-azure-project/master/k8-dep-svc.yml"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@47.129.207.94  sudo  kubectl delete deployment  spring-boot-k8s-deployment"    
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@47.129.207.94 sudo kubectl apply -f k8-dep-svc.yml"
                    }
             }
         }
         
         
    }
}

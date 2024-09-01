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
                sh 'sudo docker build -t ayyappanprofile/javatestapplication:latest .' 
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
                sh 'sudo docker push ayyappanprofile/javatestapplication:latest'
             }
         }
         
         stage('Deploy Docker in Prod using Kubernetes.')
         {
             steps
             {
                 sshagent(['dbe5a0ab-35c3-41e1-9884-63fc42b92a5a']) {
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@18.142.180.31 sudo wget https://raw.githubusercontent.com/ayyappanprofile2021/java-azure-project/master/k8-dep-svc.yml"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@18.142.180.31 sudo kubectl apply -f k8-dep-svc.yml"
                    }
             }
         }
         
         
    }
}

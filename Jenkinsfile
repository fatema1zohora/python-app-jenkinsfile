node{
   stage('SCM Checkout'){
       git 'https://github.com/fatema1zohora/python-app-jenkinsfile.git'
   }
   stage('build Docker Image'){
     sh 'docker build -t fatema1zohora/my-testpython:2.0.5 .'
   }
   stage('Push Docker Image'){
     withCredentials([string(credentialsId: 'dockerpassword', variable: 'dockerhubpassword3')]) {
        sh "docker login -u fatema1zohora -p ${dockerhubpassword3}"
     }
     sh 'docker push fatema1zohora/my-testpython:2.0.5'
   }
   stage('Run Container on Staging'){ 
     def dockerRun = 'docker run  -p 6378:6379 -d --name redis redis'
     def dockerRun1 = 'docker run -p 4042:80 -d --link redis --name my-python-app fatema1zohora/my-testpython:2.0.4'
     def dockerRun2 = 'docker rm -f my-python-app'
     def dockerRun3 = 'docker rm -f redis'
     sshagent(['dockerserver3']) {
       sh "ssh -o StrictHostKeyChecking=no root@192.168.0.23 ${dockerRun3}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.0.23 ${dockerRun2}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.0.23 ${dockerRun}"
       sh "ssh -o StrictHostKeyChecking=no root@192.168.0.23 ${dockerRun1}"
         
     }
   }
}

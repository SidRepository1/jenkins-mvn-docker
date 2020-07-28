node{
  stage ('scm checkout') {
      git ('https://github.com/SidRepository1/jenkins-mvn-docker.git')
  }

  stage ('docker image build') {
     sh 'mvn clean install dockerfile:build '
  }

  stage ('Push Docker image to DockerHub') {
   // withCredentials([usernamePassword(credentialsId: 'amazon', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
    withCredentials([string(credentialsId: 'dockerhubaccount', variable: 'dockerhubaccount')]) 
    {
     sh "docker login -u sidbhatt94 -p ${dockerhubaccount}"
      //sh "docker login -u suhvas -p Vastre@123"
    }
    sh 'docker push sidbhatt94/devops:0.1.0'
    sh 'docker rmi sidbhatt94/devops:0.1.0'
  }
  
  stage ('Deploy to Dev') {
		def dockerRun = 'docker run -p 5000:8080 -d sidbhatt94/devops:0.1.0'
		/*sshagent(['deploy-to-dev-docker']) {
			sh "ssh -o StrictHostKeyChecking=no suhvas@100.26.227.218 ${dockerRun}"
		}*/
    sh "${dockerRun}"
	}
  
  stage ('Test') {
    sh "curl http://localhost:5000"
   // sh "curl htpp://100.24.244.5:5000"
  }
}

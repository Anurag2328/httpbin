pipeline {
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('dockerhub-anurag')
    }
    stages {
        stage('checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'Anurag', url: 'https://github.com/postmanlabs/httpbin.git']]])
            }
        }
        stage('docker Image build') {
            steps {
                sh 'docker build -t anuragvashishth/test:1.0 .'
            }
        }
        stage('Login to DockerHub') {
			steps {
			    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
			}
		}

		stage('Push Docker Image to DockerHub'){
			steps {
			    withCredentials([string(credentialsId: 'dockerhub-Pwd', variable: 'dockerhubPwd')]) {
                sh "docker login -u anuragvashishth -p ${dockerhubPwd}"
                }
			    sh 'docker push anuragvashishth/test:1.0'
			}
		}

		stage('Deploy Docker Image on remote server'){
		    steps {
//		    def dockerPull = 'docker image pull anuragvashishth/test:1.0'
		    sshagent(['deploy-server']) {
              sh 'ssh -o StrictHostKeyChecking=no ubuntu@3.111.214.171 sudo docker image pull anuragvashishth/test:1.0'
            }
		    }
		}
    }
}

pipeline {
    agent any
    tools {
        maven 'maven_3_8_5'
    }

    stages {
        stage('Build Maven') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/ThanhTB/devops-automation']]])
                sh 'mvn clean install'
            }
        }

        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t vuvanthanhtb/devops-integration:2022.06.13 .'
                }
            }
        }

        stage('Push image to Hub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-login', variable: 'dockerlogin')]) {
                        sh 'docker login -u vuvanthanhtb -p ${dockerlogin}'
                    }
                    sh 'docker push vuvanthanhtb/devops-integration:2022.06.13'
                }
            }
        }
    }
}

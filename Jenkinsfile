pipeline {
    agent { label 'docker-agent'} 

    environment {
        DOCKER_IMAGE = "orvencasido/project-1"
    }

    stages {
        stage('Build') {
            steps {
                script {
                    sh "docker build -t ${DOCKER_IMAGE} ."
                }
            }
        }

        stage('Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    script {
                        sh "echo ${PASS} | docker login -u ${USER} --password-stdin"
                        sh "docker push ${DOCKER_IMAGE}"
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "docker rm -f project-1 || true"
                    sh "docker run -d -p 80:80 --name project-1 ${DOCKER_IMAGE}"
                }
            }
        }
    }
}
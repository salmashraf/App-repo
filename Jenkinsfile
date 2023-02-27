pipeline {
    agent any

    stages {
        stage('CI') {
            steps {
                git 'https://github.com/salmashraf/App-repo.git'
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh """
                docker build . -f dockerfile -t salashraf/devopsapp:v3 --network host
                 docker login -u ${USERNAME} -p ${PASSWORD}
                 docker push salashraf/devopsapp:v3
                """
                }
            }
        }
         stage('CD') {
            steps {
                git 'https://github.com/salmashraf/App-repo.git'
                withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh """
                kubectl apply -f /var/jenkins_home/workspace/backend/deploy1.yaml
                kubectl apply -f /var/jenkins_home/workspace/backend/lb.yaml
                """
                }
            }
        }
    }
}

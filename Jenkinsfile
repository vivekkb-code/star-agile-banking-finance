pipeline {
    agent any
    stages {
        stage('Build Maven') {
            steps {
                git url: 'https://github.com/vivekkb-code/star-agile-banking-finance.git', branch: "master"
                sh 'mvn clean install'
            }
        }

        stage('Build docker image') {
            steps {
                script {
                    sh 'docker build -t vivekkallate/staragilefinanceproject:v1 .'
                }
            }
        }

        stage('Docker login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push vivekkallate/staragilefinanceproject:v1'
                }
            }
        }
    }
}

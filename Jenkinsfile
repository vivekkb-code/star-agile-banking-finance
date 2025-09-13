pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git url: 'https://github.com/vivekkb-code/star-agile-banking-finance.git', branch: "master"
            }
        }

        stage('Build Maven') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker build -t vivekkallate/staragilefinanceproject:v1 .'
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

        stage('Execute Ansible Playbook') {
            steps {
                ansiblePlaybook(
                    credentialsId: 'ansible-ssh',
                    disableHostKeyChecking: true,
                    installation: 'ansible2',
                    inventory: 'ansible/inventory.ini',
                    playbook: 'ansible/application.yml',
                    vaultTmpPath: ''
                )
            }
        }
    }
}

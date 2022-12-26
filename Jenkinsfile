pipeline {
    agent any

    stages {

        stage('Initial Cleanup') {
            steps {
            dir("${WORKSPACE}") {
              deleteDir()
                }
            }
        }

        stage ('Checkout Repo'){
            steps {

                git branch: 'main', url: 'https://gitlab.com/Kingkellee/php-todo'
            }
        }

        stage ('Build Docker Image') {
            steps {
                script {

                       sh "sudo docker build -t kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage ('Run Container') {
            steps {
                script {

                       sh "sudo docker run --network php -p 8090:8000 -d kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage ('Test-Stage-Curl') {
            steps {
                script {

                    sh "curl --version"
                    sh  "curl -I http://localhost:8090"
                }
            }
        }


        stage ('Push Docker Image') {
            steps{
                script {
            sh "docker login -u ${env.username} -p ${env.password}"

            sh "sudo docker push kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                }
            }
        }


        stage ('Clean Up') {
            steps{
                script {

            sh "sudo docker system prune -af"

                }
            }
        }


        stage ('logout Docker') {
            steps {
                script {

                    sh "sudo docker logout"

                }
            }
        }

    }
}
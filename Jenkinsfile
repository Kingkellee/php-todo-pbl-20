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

                git branch: 'main', url: 'https://github.com/Kingkellee/php-todo-pbl-20.git'
            }
        }

        stage ('Build Docker Image') {
            steps {
                script {

                       sh "docker build -t kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                }
            }
        }

        stage ('Run Container') {
            steps {
                script {

                       sh "docker run --network php -p 8090:8000 -d kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
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

            sh "docker push kingkellee/php-todo:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                }
            }
        }


        stage ('Clean Up') {
            steps{
                script {

            sh "docker system prune -af"

                }
            }
        }


        stage ('logout Docker') {
            steps {
                script {

                    sh "docker logout"

                }
            }
        }

    }
}
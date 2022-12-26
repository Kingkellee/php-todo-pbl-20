pipeline {
    agent any
        environment {
            registry = "kingkellee/php-todo"
            registryCredential = 'dockerhub_id'
            dockerImage = ''
        }
        stages {
            stage("Workspace Cleanup") {
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

            stage('Building Docker image') {
                steps{
                    script {
                    dockerImage = docker.build registry +  :"$BRANCH_NAME-$BUILD_NUMBER"
                }
            }
            }
                
            stage ('Run Container') {
                steps {
                    script {

                        sh "docker run --network php -p 8050:8000 -d  $registry:$BRANCH_NAME-$BUILD_NUMBER"
                    }
                }
            }

            stage ('Test-Stage-Curl') {
                steps {
                    script {

                        sh "curl --version"
                        sh  "curl -I http://localhost:8050"
                    }
                }
            }


            stage('Deploy our image') {
                steps{
                    script {
                            docker.withRegistry( '', registryCredential ) {
                            dockerImage.push()
                        }
                    }   
                }
            }

            stage('Cleaning up') {
                steps{
                    sh "docker rmi $registry:$BUILD_NUMBER"
                }
            }
        }
}
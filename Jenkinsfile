pipeline {
    environment {
        registry = "kingkellee/php-todo"
        registryCredential = 'dockerhub_id'
        dockerImage = ''
    }
    agent any
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

                    git branch: 'main', url: 'https://gitlab.com/Kingkellee/php-todo'
                }
            }

            stage('Building Docker image') {
                steps{
                    script {
                    dockerImage = docker.build registry + :${env.BRANCH_NAME}-${env.BUILD_NUMBER}
                }
            }
            }
                
            stage ('Run Container') {
                steps {
                    script {

                        sh "docker run --network php -p 8050:8000 -d  $registry:${env.BRANCH_NAME}-${env.BUILD_NUMBER} ."
                    }
                }
            }

            stage ('Test-Stage-Curl') {
                steps {
                    script {

                        sh "curl --version"
                        sh  "curl -I http://3.95.65.147:8050"
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
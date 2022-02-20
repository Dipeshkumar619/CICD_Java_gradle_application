pipeline{
    agent any
    environment {
        VERSION="${env.BUILD_NUMBER}"
    }
    stages{
        stage("Sonar Quality Check"){
            agent {
                docker {
                    image 'openjdk:11'
                }
            }

            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube_token') {    
                        sh 'chmod 777 gradlew'
                        sh 'java -version'
                        sh './gradlew sonarqube'
                    }
                    
                     timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }
                }
            }

        }

        stage("Docker build and Docker Push"){
            steps{
                withCredentials([string(credentialsId: 'nexus_password', variable: 'nexus_password')]) {
                sh '''
                docker build -t 3.236.136.57:8083/springapp:${VERSION} .
                docker login -u admin -p ${nexus_password} 3.236.136.57:8083
                docker push 3.236.136.57:8083/springapp:${VERSION}
                docker rmi 3.236.136.57:8083/springapp:${VERSION}
                '''
                }

            }

        }

    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}
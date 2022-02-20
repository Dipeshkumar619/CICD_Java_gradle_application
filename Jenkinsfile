pipeline{
    agent any
    environment {
        VERSION="${env.BUILD_NUMBER}"
    }
    stages{
        stage("Sonar Quality Check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonarqube_token') {
                        sh 'chmod 777 gradlew'
                        sh './gradlew sonarqube'
                    }

                timeout(activity: true, time: 5) {
                    def qg=waitForQualityGate()
                    if (qg.status!="OK"){
                        error "pipeline failed due to quality gate failure: ${qg}"
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
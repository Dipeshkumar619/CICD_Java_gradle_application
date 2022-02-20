pipeline{
    agent any
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
                        error "pipeline failed due to quality gate failure: ${qg.status}"
                    }
                }
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
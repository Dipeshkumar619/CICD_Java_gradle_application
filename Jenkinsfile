pipeline{
    agent any
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
                        sh 'chmod +X gradlew'
                        sh './gradlew sonarqube'
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
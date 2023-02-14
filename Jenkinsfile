pipeline{
    agnet any
    stages{
        stage("Sonar Quality Check"){
            agent{
                docker {
                    image "openjdk:11.0"
                }
            }
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
   
                }

                }
            }

        }
    }
}
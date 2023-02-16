pipeline{
    agent any
    stages{
        stage("Sonar Quality check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-auth') {
                    sh 'chmod +x gradlew'
                    sh './gradlew sonarqube'
}

                }
            }
        }
    }
}
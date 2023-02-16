pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
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
        stage("Sonar Quality Gate Status"){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-auth'
                }
            }
        }
        stage ("Docker image Build and push "){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker-pass', variable: 'docker-auth')]) {
                        sh '''
                           docker image build -t 65.0.205.164:8081/springapp:$VERSION .
                           docker login -u admin -p $docker-auth 65.0.205.164:8083
                           docker push 65.0.205.164:8081/springapp:$VERSION
                           docker rmi 65.0.205.164:8081/springapp:$VERSION
                           

                    '''
    
}
                    
                }
            }
        }
    }
}

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
                    withCredentials([string(credentialsId: 'docker_password', variable: 'docker_auth')]) {
                          sh '''
                           docker image build -t 65.0.205.164:8083/springapp:$VERSION .
                           docker login -u admin -p $docker_auth 65.0.205.164:8083
                           docker push 65.0.205.164:8083/springapp:$VERSION
                           docker rmi 65.0.205.164:8083/springapp:$VERSION
                           '''
                           } 
                        }
                    }
            }
            stage("Identifying misconfigs using datree in Helm Charts"){
                steps{
                    script{
                        dir('kubernetes/') {
                            sh ' helm datree test /myapp'
    
}
                    }
                }
            }

    }
    post {
		always {
			mail bcc: '', body: "<br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "${currentBuild.result} CI: Project name -> ${env.JOB_NAME}", to: "aniljoshi.synsoft@gmail.com";  
		}
	}
}

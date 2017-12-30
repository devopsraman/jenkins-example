pipeline {
    agent any

    stages {
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven_3_5_2') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven_3_5_2') {
                    sh 'mvn test'
                }
            }
        }
        
       stage ('build & SonarQube Scan') {
            steps {
                withSonarQubeEnv('My SonarQube Server') {
                        sh 'mvn clean package sonar:sonar'
                }
            }// SonarQube taskId is automatically attached to the pipeline context
        }
        stage ("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') { 
                    def qg = waitForQualityGate() 
                    if (qg.status != 'OK') {
                       error "Pipeline aborted due to quality gate failure: ${qg.status}"
                   }
                }
           }
         }
        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_5_2') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}

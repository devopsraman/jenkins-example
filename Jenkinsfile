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
        
         stage("build & SonarQube analysis") {
             steps {
              withSonarQubeEnv('Sonar') {
                  withMaven(maven : 'maven_3_5_2') {
                     sh 'mvn clean package sonar:sonar'
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
stage("Quality Gate"){
          timeout(time:1, unit: 'MINUTES') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }
      }

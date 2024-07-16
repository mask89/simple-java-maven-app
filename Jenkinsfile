pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '/usr/local/maven/bin/mvn -B -DskipTests clean package'
            }
        }
        stage('Test') { 
            steps {
                sh '/usr/local/maven/bin/mvn test' 
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml' 
                }
            }
        }
        stage("build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('sq1') {
                sh '/usr/local/maven/bin/mvn clean package sonar:sonar'
              }
            }
        }
          stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
    }
}
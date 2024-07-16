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
                sh '/usr/local/maven/bin/mvn clean package sonar:sonar -Dsonar.projectKey=Demo -Dsonar.projectName='Demo' -Dsonar.host.url=http://172.18.25.204:9000/ -Dsonar.token=sqp_014bd86730736b7de34fd2bf877c0aef956bc53e'
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
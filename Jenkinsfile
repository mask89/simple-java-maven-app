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
        stage('Dependency Check') {
            steps {
                dependencyCheck additionalArguments: '--format HTML', odcInstallation: 'DP-Check'
            }
        }
        stage("Build & SonarQube analysis") {
            agent any
            steps {
              withSonarQubeEnv('sq1') {
                sh "/usr/local/maven/bin/mvn clean package sonar:sonar -D sonar.projectKey=Demo -D sonar.projectName='Demo' -D sonar.host.url=http://172.18.25.204:9000/ -D sonar.token=sqp_014bd86730736b7de34fd2bf877c0aef956bc53e"
              }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 5, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
    }
}
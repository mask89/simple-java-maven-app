pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh '/usr/local/maven/bin/mvn -B -DskipTests clean package' 
            }
        }
    }
}
pipeline {
    agent any 
     tools { 
        maven 'Maven 3.6.3' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
        stage('Sonar Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectName=simplicity -Dsonar.host.url=${Sonar_URL} -Dsonar.login=${Simplicity}'
            }
        }
    }
}

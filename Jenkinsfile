pipeline {
    agent any 
     tools { 
        maven 'MAVEN' 
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }
         stage('Dependencies Check') {
            steps {
                sh 'mvn org.owasp:dependency-check-maven:aggregate'
            }
        } 
        stage('Sonar Analysis') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectName=simplicity -Dsonar.host.url=${Sonar_URL} -Dsonar.login=${Simplicity}'
            }
        }
    }
}

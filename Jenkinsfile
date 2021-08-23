pipeline {
    agent {
        docker {
            image 'eddevopsd2/maven-java-npm-docker:mvn3.6.3-jdk8-npm6.14.4-docker'
            args '-v /root/.m2:/root/.m2'
        }
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
    }
}
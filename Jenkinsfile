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
                sh '/opt/sonar-scanner/bin/sonar-scanner -Dsonar.projectName=simplicity -Dsonar.host.url=${Sonar_URL} -Dsonar.login=${Simplicity}'
            }
        }
        stage('Produce bom.xml'){
             steps{
                sh 'mvn org.cyclonedx:cyclonedx-maven-plugin:makeAggregateBom'
            }
        }
        stage('Dependency-Track Analysis'){
            steps{
                sh '''
                    cat > payload.json <<__HERE__
                {
                "project": "09d39268-5cb8-4b6b-947f-afb96dce1354",
                "bom": "$(cat target/bom.xml |base64 -w 0 -)"
                }
                 __HERE__
                    '''

                 sh     '''
                 curl -X "PUT" ${DEPENDENCY_TRACK_URL} -H 'Content-Type: application/json' -H 'X-API-Key: '${DEPENDENCY_TRACK_API_KEY} -d @payload.json
                '''
            }
        }
    }
}

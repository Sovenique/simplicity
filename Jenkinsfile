pipeline {
   
   agent {
        docker {
            image 'custom-jdk11-jdk8'
            args '-v /root/.m2:/root/.m2'
        }
    }
    stages {
        stage('File transfer'){
            steps{
                sh('scp -i ~/.ssh/id_rsa.pub /root/myfile.txt root@devops-test-1.eurodyn.com:/var')
            }
        }
        stage('Build') {
            steps {
                sh 'update-java-alternatives -s java-1.8.0-openjdk-amd64'
                sh 'mvn clean install'
                sh 'java -version'
            }
        }
          stage('Dependencies Check') {
            steps {
                sh 'java -version'          
                sh 'mvn org.owasp:dependency-check-maven:aggregate'
            }
        }
        stage('Sonar Analysis') {
            steps {
                sh 'java -version'
                sh 'mvn sonar:sonar -Dsonar.projectName=Simplicities -Dsonar.host.url=${Sonar_URL} -Dsonar.login=${Simplicity}'
            }
        }
        stage('Produce bom.xml'){
             steps{
                sh 'java -version'
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
                 curl -X "PUT" http://cicd-dt.eurodyn.com/api/v1/bom -H 'Content-Type: application/json' -H 'X-API-Key: 'W6F6Lh73RK2JJ5RN1Gxy4Zif4IUDET1r -d @payload.json
                '''
            }
        }
    }
}
//test 2
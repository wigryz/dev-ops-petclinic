pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'ls -a'
                script {
                    def testImage = docker.build("petclinic", "Dockerbuild")
                }
            }
// --             post {
// --                 // If Maven was able to run the tests, even if some of the test
// --                 // failed, record the test results and archive the jar file.
// --                 success {
// --                     junit '**/target/surefire-reports/TEST-*.xml'
// --                     archiveArtifacts 'target/*.jar'
// --                 }
// --             }
        }
        stage('Test') {
            steps {
                sh 'echo testing'
                sh 'ls -a'
            }
        }
    }
}

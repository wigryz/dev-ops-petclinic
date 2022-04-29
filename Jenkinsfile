pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'git@github.com:wigryz/dev-ops-petclinic.git'

                // Run Maven on a Unix agent.
                sh "./mvnw -Dmaven.test.failure.ignore=true clean package"

            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}

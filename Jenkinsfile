pipeline {
    agent any

    stages {
        stage("Fetch dependencies") {
            steps {
                script {
                    docker.build("wigryz/petclinic-dep", ". -f Dockerdep")
                    sh 'echo dependencies fetched'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    docker.build("wigryz/petclinic-build", ". -f Dockerbuild")
                    sh 'echo builded'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.build("wigryz/petclinic-test", ". -f Dockertest")
                    sh 'echo tested'
                }
            }
        }
        stage('Publish') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
                        def publishImage = docker.build("wigryz/petclinic", ". -f Dockerpublish")
                        publishImage.push('latest')
                    }
                    sh 'echo published'
                }
            }
        }
    }
}

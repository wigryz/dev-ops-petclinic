pipeline {
    agent any

    stages {
        stage("Fetch dependencies") {
            steps {
                script {
                    docker.build("petclinic-dep", ". -f Dockerdep")
                    sh 'echo dependencies fetched'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    sh 'ls'
                    def imageBuild = docker.build("petclinic-build", ". -f Dockerbuild")
                    sh 'rm -rf shared_volume'
                    sh 'mkdir shared_volume'
                    imageBuild.run("-v /$(pwd)/shared_volume/output")
                    sh 'echo builded'
                    sh 'ls shared_volume'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    docker.build("petclinic-test", ". -f Dockertest")
                    sh 'echo tested'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def deployImage = docker.build("petclinic", ". -f Dockerpublish")
                    deployImage.run("--mount 'source=output,destination=/output'")
                }
            }
        }
        stage('Publish') {
            steps {
                script {
                    def image = docker.image("petclinic")
                    image.inside { 
                        sh "cp /app.jar ${WORKSPACE}"
                        archiveArtifacts 'app.jar'
                    }
                    sh 'echo published'
                }
            }
        }
    }
}

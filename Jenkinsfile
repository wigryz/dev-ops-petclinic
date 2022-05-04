pipeline {
    agent any

    stages {
        stage("Fetch dependencies") {
            steps {
                script {
                    docker.build('petclinic-dep", ". -f Dockerdep")
                    sh 'echo dependencies fetched'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    def imageBuild = docker.build("petclinic-build", ". -f Dockerbuild")
                    sh 'volume create --name output'
                    imageBuild.run("--mount 'source=output,destination=/output'")
                    sh 'echo builded'
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

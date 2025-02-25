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
                    imageBuild.run("-v \$(pwd)/shared_volume:/output")
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
                    def deployImage = docker.build("wigryz/petclinic", ". -f Dockerpublish")
                    deployImage.run("--name petclinic")
                    sh 'sleep 5'
                    sh 'docker rm -f petclinic'
                    
                    withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
                        deployImage.push('latest')
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Succeeded, now I`m saving artifact.'
            archiveArtifacts artifacts: 'shared_volume/app.jar', fingerprint: true
        }
        failure {
            echo 'Failed, I`m not saving any artifacts.'
        }
    }
}

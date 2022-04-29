pipeline {
    agent any

    stages {
        stage("Fetch dependencies") {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
                        def dependenciesImage = docker.build("wigryz/petclinic-dep", ". -f Dockerdep")
                        dependenciesImage.push('latest')
                    }
                    sh 'echo dependencies fetched'
                }
            }
        }
        
        stage('Build') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
                        def buildImage = docker.build("wigryz/petclinic", ". -f Dockerbuild")
                        buildImage.push('latest')
                    }
                    sh 'echo builded'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    withDockerRegistry([credentialsId: 'docker-hub', url: ""]) {
                        def testImage = docker.build("wigryz/petclinic-tested", ". -f Dockertest")
                        testImage.push('latest')
                    }
                    sh 'echo tested'
                }
            }
        }
    }
}

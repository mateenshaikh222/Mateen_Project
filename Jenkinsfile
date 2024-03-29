@Library('my-shared-library') _

pipeline {
    agent any

    parameters {
        choice(name: 'action', choices: 'Create\nDelete', description: 'choose Create/Destroy')
        string(name: 'ImageName', description: "name of the docker", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "tag of the docker build", defaultValue: 'v1')
        string(name: 'DockerHubUser', description: "name of the Application", defaultValue: 'mateenshaikh00')
    
    }

    stages {
        stage('Git Checkout') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    gitCheckout(
                        branch: "main",
                        url: "https://github.com/Mateenshaikh000111/Mateen_Project.git"
                    )
                }
            }
        }

        stage('Unit Test Maven') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    script {
                        mvnTest()
                    }
                }
            }
        }

        stage('Integration Test Maven') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    script {
                        mvnintegrationTest()
                    }
                }
            }
        }

        stage('Static Code Analysis Sonarqube') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    script {
                        def SonarQubecredentialsId = 'SonarQube_API'
                        StaticCodeAnalysis(SonarQubecredentialsId)
                    }
                }
            }
        }

        stage('Quality gate status') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 20, unit: 'MINUTES') {
                    script {
                        def SonarQubecredentialsId = 'SonarQube_API'
                        QualityGateStatus(SonarQubecredentialsId)
                    }
                }
            }
        }

        stage('Maven Build : maven') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {
                        mvnBuild()
                    }
                }
            }
           }
        stage('Docker image Build') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {

                        dockerBuild("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                    }
                }
            }
             }
        stage('Docker image Scan ') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {

                        dockerimageScan("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                    }
                }
            }
        }
                stage('Docker image Push : Dockerhub') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {

                        dockerImagePush("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                    }
                }
            }
        }
                        stage('Docker image CleanUP ') {
            when {
                expression { params.action == 'Create' }
            }
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    script {

                        dockerimageCleanup("${params.ImageName}","${params.ImageTag}","${params.DockerHubUser}")
                    }
                }
            }
        }
    }
}




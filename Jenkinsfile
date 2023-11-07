pipeline {
    agent {
        label 'slave'
    }
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerCredential')   
    }
    stages {
        stage('Hello') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[credentialsId: 'gitupToken', url: 'https://github.com/saravana1955/todo.git']]
                )
            }
        }

        stage('Find and Build React Project') {
            steps {
                script {
                    echo "Current PATH: ${env.PATH}"
                    dir('/Users/apple/.jenkins/workspace/wehooks-test') {
                        echo "Jenkins workspace path: ${env.WORKSPACE}"
                        // npm 'install --force'
                        // npm 'run build'
                        def customImage = docker.build("saravana19/angular1245:${env.BUILD_ID}")
                    }
                }
            }
        }
        stage('Push to Docker Hub') {
            steps {
                script {
                    def customImage = 'saravana19/angular1245'
                    withCredentials([usernamePassword(credentialsId: 'dockerCredential', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                        sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                        sh "docker push ${customImage}:${env.BUILD_ID}"
                    }
                }
            }
        }

         stage('Check AWS Credentials') {
                    steps {
                        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws']]) {
                            sh 'aws s3 ls'
                            sh 'aws s3 rm s3://my-bucket-provility/ --recursive'
                             sh 'aws s3 sync /Users/apple/.jenkins/workspace/wehooks-test/build/ s3://my-bucket-provility/'
                        }
                    }
                }
    }
}

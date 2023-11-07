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
                // Checking out the code from the Git repository
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
                    // Change to the project directory
                    dir('/Users/apple/.jenkins/workspace/wehooks-test') {
                        echo "Jenkins workspace path: ${env.WORKSPACE}"
                    
                        // npm 'install --force'
                        // npm 'run build'
                        def customImage = docker.build("saravana19/angular1245:${env.BUILD_ID}")
                        // docker push saravana19/angular1245:v1
                    }
                }
            }
        }
        
        stage('Pull from Docker Hub') {
            steps {
                script {
                    sh "docker pull saravana19/angular"
                }
            }
        }
    

//       stage('Login to Docker Hub') {      	
//     steps{                       	
// 	sh 'echo $DOCKERHUB_CREDENTIALS_PSW | sudo docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'                		
// 	echo 'Login Completed'      
//     }           
// }
stage('Push to Docker Hub') {
            steps {
                script {
                   
                    def customImage = 'saravana19/angular1245'
                    //   docker login -u $USERNAME --password-stdin
                    //   withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId:          env.DOCKERHUB_CREDENTIALS, usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                    // //   docker login -u $USERNAME --password-stdin
                    //   echo "saravana"
                    //   docker login -u saravana19 -p 123456789
                    //     // sh "docker tag saravana19/angular1245:v1 $customImage" // Correct the image name to tag
                    //     // sh "docker push $customImage"
                    // }
      	withCredentials([usernamePassword(credentialsId: 'dockerCredential', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
        	 sh "docker push ${customImage}:${env.BUILD_ID}"
        }
                }
                    
                }
            }
        }


        // Other stages of your pipeline...
    }


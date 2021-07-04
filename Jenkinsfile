pipeline{
  environment {
    registry = "rickyuby/hellojenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any

    tools {
        nodejs "nodejs"
        dockerTool "docker"
        
    }

    stages {
        stage('Build'){
            steps{
                script{
                    sh 'npm install'
                }
            }
        }

        stage('Building image') {
            steps{
                script {
                  dockerImage = docker.build registry + ":latest"
                }
             }
          }

          stage('Push Image') {
              steps{
                  script 
                    {
                            docker.withTool('docker'){
                        docker.withRegistry( '', registryCredential ) {
                            dockerImage.push()
                        }
                        }
                   } 
               }
            }

        stage('Deploying into k8s'){
            steps{
                sh 'kubectl apply -f kube-hellojenkins.yml'
            }
        }
    }
}

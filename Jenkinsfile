pipeline{
    agent any
    
    stages{
        stage('Docker Build'){     
            steps{
                sh "docker build . -t $JOB_NAME:v1.$BUILD_ID"
                sh "docker tag $JOB_NAME:v1.$BUILD_ID bijuvinoth/$JOB_NAME:v1.$BUILD_ID"
                sh "docker tag $JOB_NAME:v1.$BUILD_ID bijuvinoth/$JOB_NAME:latest"  
            }
        }
        stage('DockerHub Push'){
            steps{
                withCredentials([string(credentialsId: 'docker_hub', variable: 'docker_hub')]) {
                    sh "docker login -u bijuvinoth -p ${docker_hub}"
                }
                sh "docker push bijuvinoth/$JOB_NAME:v1.$BUILD_ID"
                sh "docker push bijuvinoth/$JOB_NAME:latest"
                sh "docker rmi $JOB_NAME:v1.$BUILD_ID bijuvinoth/$JOB_NAME:v1.$BUILD_ID bijuvinoth/$JOB_NAME:latest"
            }
        }
        stage('Docker Deploy'){
            steps{
              ansiblePlaybook credentialsId: 'Docker-server', disableHostKeyChecking: true, installation: 'ansible', inventory: 'dev.inv', playbook: 'deploy-docker.yml'
            }
        }
    }    
}

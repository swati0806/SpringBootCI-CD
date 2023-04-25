pipeline {
    agent any
    tools{
        maven 'maven'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/swati0806/SpringBootCI-CD.git']])
                sh 'mvn clean install'
            }
        }
     stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t swatinamdev/cicdkube .'
                }
            }
        }
     stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubpwd')])  {
                    sh 'docker login -u swatinamdev -p ${dockerhubpwd}'
                        
                    }
                    sh 'docker push swatinamdev/cicdkube'
                }
            }
        }
    stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    }
}

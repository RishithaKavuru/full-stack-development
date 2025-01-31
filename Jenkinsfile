// jenkinsfile
pipeline{
    agent any
    environment {
        DOCKERHUB_PASS = credentials('docker')
    }
    stages {
        stage("Building the Student Survey Image") {
            steps {
                script {
                    checkout scm
                    
                    sh 'rm -rf *.war'
                    sh 'jar -cvf newhw3.war -C 645HW3 .'
                    sh 'echo ${BUILD_TIMESTAMP}'
                    sh "docker login -u $DOCKERHUB_PASS_USR -p $DOCKERHUB_PASS_PSW"
                    sh "docker build -t meghanakancherla/survey_hw3:${BUILD_TIMESTAMP} ."

                }
            }
        }
        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    sh 'docker push meghanakancherla/survey_hw3:${BUILD_TIMESTAMP}'
                }
            }
        }
        stage("Deploying to Rancher as single pod") {
            steps{
                script {
                    def kubeconfigPath = "clust2.yaml"
                    env.KUBECONFIG = kubeconfigPath
                    sh "kubectl set image deployment/hw3nodeport container-0=meghanakancherla/survey_hw3:${BUILD_TIMESTAMP} -n hw3namespace"
                }
            }
        }
        stage("Deploying to Rancher as load balancer"){
            steps {
                sh "kubectl set image deployment/hw3loadbalancer container-0=meghanakancherla/survey_hw3:${BUILD_TIMESTAMP} -n hw3namespace"
                sh "kubectl cluster-info"
            }
        }
    }
}

pipeline {
    agent any
    environment {
        PROJECT_ID = 'PROJECT-ID'
        CLUSTER_NAME = 'kube1'
        LOCATION = 'CLUSTER-LOCATION'
        CREDENTIALS_ID = 'local_kube'
    }
    stages {
        stage("Checkout code") {
            steps {
                checkout scm
            }
        }
        stage("Build image") {
            steps {
                script {
                    sh "docker login -u giaple -p P@ssw0rd123456"
                    myapp = docker.build("giaple/gotest:${env.BUILD_ID}")
                }
            }
        }
        stage("Push image") {
            steps {
                script {
                    sh """
                    docker login -u giaple -p P@ssw0rd123456
                    docker push giaple/gotest:${env.BUILD_ID}
                    docker rmi giaple/gotest:${env.BUILD_ID}
                    """
                }
            }
        }        
        stage('Deploy to Kubenetes-Local') {
            steps{
                sh "sed -i 's/gotest:latest/gotest:${env.BUILD_ID}/g' deployment.yaml"
                sh "kubectl apply -f deployment.yaml"
            }
        }
    }    
}
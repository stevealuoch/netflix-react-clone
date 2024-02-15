pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages {
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
        
        stage('build Docker Image') {
            steps {
                script {
                    // build image
                    def myImage = docker.build("335871625378.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest")
                }
            }
        }
        
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 335871625378.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest'
            }
        }
        
        stage('Push to ECR') {
            steps {
                script {
                    // Using the credential ID provided
                    docker.withRegistry('https://335871625378.dkr.ecr.us-east-1.amazonaws.com', 'stevealuoch-ecr') {
                        // No need to build the image again, just push
                        def myImage = docker.image("335871625378.dkr.ecr.us-east-1.amazonaws.com/netflix-jan:latest")
                        myImage.push()
                    }
                }
            }
        }
    }
}

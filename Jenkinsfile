pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-id'   // Jenkins credentials ID for DockerHub
        DOCKERHUB_USERNAME = 'salhianis20'
        SONARQUBE_ENV = 'SonarQube'
        SONAR_PROJECT_KEY = 'Microservices-App'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Scan') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'SonarQube-ID', variable: 'jenkins')]) {
                        withSonarQubeEnv("${env.SONARQUBE_ENV}") {
                            sh """
                                sonar-scanner \
                                -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                                -Dsonar.sources=. \
                                -Dsonar.token=${jenkins}
                            """
                        }
                    }
                }
            }
        }

        stage('Build, Scan and Push Images') {
            steps {
                script {
                    def services = ['auth', 'client', 'expiration', 'orders', 'payments', 'tickets']
        
                    withCredentials([usernamePassword(credentialsId: env.DOCKERHUB_CREDENTIALS,
                                      usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
        
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
        
                        for (service in services) {
                            def imageName = "${DOCKERHUB_USERNAME}/${service}:latest"
        
                            echo "Building image for ${service} with tag latest"
                            sh "docker build -t ${imageName} ./${service}"
        
                            echo "Scanning image ${imageName} with Trivy"
                            // Run Trivy scan but do not fail the build on vulnerabilities:
                            sh "trivy image --severity HIGH,CRITICAL ${imageName} || true"
        
                            echo "Pushing image ${imageName}"
                            sh "docker push ${imageName}"
                        }
        
                        sh "docker logout"
                    }
                }
            }
        }
    }
}

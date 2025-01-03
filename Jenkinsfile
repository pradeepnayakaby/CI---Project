pipeline {
    agent any
    tools {
        jdk "java-11"
        maven "maven"
    }
    stages {
        stage("Git Checkout") {
            steps {
                git branch: "main", url: "https://github.com/pradeepnayakaby/CI---Project.git"
            }
        }
        stage("Compile") {
            steps {
                sh "mvn compile"
            }
        }
        stage("Build") {
            steps {
                sh "mvn install"
            }
        }
        stage("Build and Tag Docker file") {
            steps {
                sh "docker build -t pradeepnayakaby/punithrajkumar:1 ."
            }
        }
        stage('Containerization') {
            steps {
                script {
                    // Stop and remove any existing containers with the same name
                    sh '''
                    docker stop punithrajkumar_web_page || true
                    docker rm punithrajkumar_web_page || true
                    docker run -it -d --name punithrajkumar_web_page -p 9000:8080 pradeepnayakaby/punithrajkumar:1
                    '''
                }
            }
        }
        stage("Login to Docker Hub") {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }
        stage("Push the Image to Repository") {
            steps {
                sh "docker push pradeepnayakaby/punithrajkumar:1"
            }
        }
    }
}

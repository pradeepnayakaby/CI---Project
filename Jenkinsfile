pipeline{
    agent any
    tools{
        jdk "java-11"
        maven "maven"
    }
    stages{
        stage("git chekout"){
            steps{
                git branch:"main",url:"https://github.com/pradeepnayakaby/CI---Project.git"
            }
        }
        stage("compile"){
            steps{
                sh "mvn compile"
            }
        }
        stage("build"){
            steps{
                sh "mvn install"
            }
        }
        stage("build and tag docker"){
            steps{
                sh "docker build -t pradeepnayakaby/punithrajkumar:1 ."
            }
        }
        stage('containerzion'){
            steps{
                sh '''
                docker stop /pradeepnayakaby/punithrajkumar:1
                docker rm pradeepnayakaby/punithrajkumar:1
                docker run -it -d --name punithrajkumar_web_page -p 9000:8080 pradeepnayakaby/punithrajkumar:1
                '''
            }
        }
        stage("login t docker hub"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                }
            }
        }
        stage("pushing the images to repository"){
            steps{
                sh " docker push pradeepnayakaby/punithrajkumar:1 "
            }
        }
    }
}
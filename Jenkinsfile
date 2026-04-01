pipeline {
    agent any

    tools {
        jdk 'jdk17'
    }

    environment {
        IMAGE_NAME = "nouhasd/student-management"
    }

    stages {

        stage('Build Java') {
            steps {
                dir('student-management') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

       stage('Build Docker Image') {
    steps {
        sh 'docker build -t $IMAGE_NAME -f Dockerfile student-management'
    }
}

        stage('Login DockerHub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh 'echo $PASS | docker login -u $USER --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh 'docker push $IMAGE_NAME'
            }
        }
    }
}

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
        stage('SonarQube Analysis') {
            steps {
                dir('student-management') {
                    withSonarQubeEnv('sonarqube') {
                        sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=student-management \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=$SONAR_AUTH_TOKEN
                        '''
                    }
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
        stage('Kubernetes Deploy') {
    steps {
        sh 'kubectl apply -f pv-sql.yaml --validate=false'
        sh 'kubectl apply -f pvc-sql.yaml --validate=false'
        sh 'kubectl apply -f deploy-sql.yaml --validate=false'
        sh 'kubectl apply -f service-sql.yaml --validate=false'
        sh 'kubectl apply -f configmap-spring.yaml --validate=false'
        sh 'kubectl apply -f secret-spring.yaml --validate=false'
        sh 'kubectl apply -f deploy-spring.yaml --validate=false'
        sh 'kubectl apply -f service-spring.yaml --validate=false'
    }
}
        stage('Deploy MySQL & Spring Boot on K8s') {
            steps {
                sh 'kubectl rollout status deployment/mysql-deployment -n devops'
                sh 'kubectl rollout status deployment/spring-app-deployment -n devops'
            }
        }
    }
}

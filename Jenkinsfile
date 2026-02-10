
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git https://github.com/nouhasaidanee/student-management.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }
    }
}

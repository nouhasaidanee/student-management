pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                dir('student-management/student-management') {
                    sh 'mvn clean compile'
                }
            }
        }

    }
}

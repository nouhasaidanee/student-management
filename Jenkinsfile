pipeline {
    agent any
    tools {
        jdk 'jdk17'
    }
    stages {
        stage('Debug Java') {
            steps {
                sh 'which java'
                sh 'java -version'
                sh 'which javac'
                sh 'javac -version'
            }
        }

        stage('Build') {
            steps {
                dir('student-management') {
                    sh 'mvn clean compile'
                }
            }
        }
    }
}

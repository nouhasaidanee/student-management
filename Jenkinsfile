pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                // On se place dans le dossier où se trouve pom.xml
                dir('student-management') {
                    sh 'ls -l'                 // pour vérifier qu'on voit pom.xml
                    sh 'mvn clean compile'
                }
            }
        }

    }
}

pipeline {
    agent any
    stages {
        stage('Build') {
             withMaven(maven: '3.6.3') {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            withMaven(maven: '3.6.3') {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
}

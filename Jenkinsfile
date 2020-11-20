pipeline {
    agent any
    stages {
         stage('Build') {
             steps {
                  withMaven(maven: '3.6.3') {
                     sh 'mvn -B -DskipTests clean package'
                     }
                 }
             }
          }
}

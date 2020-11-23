node {
    stage('Initialize') {
        def dockerHome = tool 'MyDocker'
        def mavenHome  = tool 'MyMaven'
        def javaHome  = tool 'MyJava'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${javaHome}/bin:${env.PATH}"
    }

  stage('Build') {
        sh 'mvn -B -DskipTests clean package -e'
      }

      stage('BuildOne') {
             steps {
               script {
                dir("test")
                  {
                   sh  'touch $WORKSPACE/Artifact_$BUILD_NUMBER'
                  }
                  }
                }
              }
               stage ('Upload') {
                  steps {
                      rtUpload (
                          buildName: JOB_NAME,
                          buildNumber: BUILD_NUMBER,
                          serverId: artifactory,
                          spec: '''{
                                    "files": [
                                       {
                                        "pattern": "$WORKSPACE/Demo-Artifactory/Artifact_*",
                                        "target": "result/",
                                        "recursive": "false"
                                      }
                                   ]
                              }'''
                          )
                  }
              }
}

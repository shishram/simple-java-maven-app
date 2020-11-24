node {
    stage('Initialize'){
        def dockerHome = tool 'MyDocker'
        def mavenHome  = tool 'MyMaven'
        def javaHome  = tool 'MyJava'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${javaHome}/bin:${env.PATH}"
    }

  stage('Build'){
        sh 'mvn -B -DskipTests clean package -e'
     }

  stage('publish'){

    def server = Artifactory.newServer "https://artifactory.axisb.com/artifactory"
    server.bypassProxy = true
    // If you're using username and password:
    server.username = '177276'
    server.password = 'Axis1234'
    def buildInfo = Artifactory.newBuildInfo()
    buildInfo.env.capture = true
    buildInfo.env.collect()
    def rtMaven = Artifactory.newMavenBuild()
    rtMaven.tool = "MyMaven" // Tool name from Jenkins configuration
    rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
    rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
    def uploadSpec = """{
        "files": [{
                    "pattern": "target/my-app*.jar",
                    "target": "libs-snapshot-local"
                }
            ]
        }"""
    server.upload(uploadSpec)
  }
}

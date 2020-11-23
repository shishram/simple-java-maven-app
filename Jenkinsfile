node {
    stage('Initialize')
    {
        def dockerHome = tool 'MyDocker'
        def mavenHome  = tool 'MyMaven'
        def javaHome  = tool 'MyJava'
        env.PATH = "${dockerHome}/bin:${mavenHome}/bin:${javaHome}/bin:${env.PATH}"
    }

  stage('Build')
       {
        sh 'mvn -B -DskipTests clean package -e'
      }

  stage('publish'){

  def server = Artifactory.server "artifactory"
    def buildInfo = Artifactory.newBuildInfo()
    buildInfo.env.capture = true
    def rtMaven = Artifactory.newMavenBuild()
    rtMaven.tool = MyMaven // Tool name from Jenkins configuration
    rtMaven.opts = "-Denv=dev"
    rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
    rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server

    rtMaven.run pom: 'pom.xml', goals: 'clean install', buildInfo: buildInfo

    buildInfo.retention maxBuilds: 10, maxDays: 7, deleteBuildArtifacts: true
    // Publish build info.
    server.publishBuildInfo buildInfo
  }


}

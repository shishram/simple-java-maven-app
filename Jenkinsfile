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
}

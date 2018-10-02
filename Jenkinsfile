builderNode {
  checkout scm
  withCredentials([usernameCredentials(id: "jenkins_lendup.com", prefix: "ARTIFACTORY")]) {
    def mavenActions = "verify"
    if ("${env.PULL_REQUEST_NUMBER}" == "") {
      mavenActions += " deploy"
    }
    stage(mavenActions) {
      if (sh(script: """
        docker run \\
          --rm \\
          -i \\
          -e ARTIFACTORY_USERNAME \\
          -e ARTIFACTORY_PASSWORD \\
          --workdir /app \\
          -v \$(pwd):/app \\
          docker.gameofloans.com/universe/maven:3.5-jdk-8 \\
            mvn ${mavenActions}
      """, returnStatus: true) != 0) {
        currentBuild.result = "UNSTABLE"
      }
      junit("target/surefire-reports/*.xml")
    }
  }
}

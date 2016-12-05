
    // Make sure that the tools we need are installed and on the path.
    def mvnHome = tool 'M3'

    // Run on executors with the "docker" label, because it's either that or Windows here.
    agent label:"node"

    // Make sure we have GIT_COMMITTER_NAME and GIT_COMMITTER_EMAIL set due to machine weirdness.
    environment {
        GIT_COMMITTER_NAME = "jenkins"
        GIT_COMMITTER_EMAIL = "jenkins@jenkins.io"
    }
    
    stage("build") {
        bat '${mvnHome}/bin/mvn clean install -Dmaven.test.failure.ignore=true'
    }
    
    
    // The order that sections are specified doesn't matter - this will still be run
    // after the stages, even though it's specified before the stages.
    post {
        // No matter what the build status is, run these steps. There are other conditions
        // available as well, such as "success", "failed", "unstable", and "changed".
        always {
            archive "target/**/*"
            junit 'target/surefire-reports/*.xml'
        }
}

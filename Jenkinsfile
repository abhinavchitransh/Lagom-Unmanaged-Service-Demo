node {
    stage('Initialization') {
        checkout scm
        echo 'this.env.BRANCH_NAME--------->'+this.env.NODE_NAME
        String branchName = env.NODE_NAME
        echo 'branchName--------->'+branchName
    }
    stage('Maven') {
        //def MAVEN_HOME = "${tool 'M3'}/bin"
        def MAVEN_HOME = "D:/softwares/apache-maven-3.6.1"
        //MAVEN_HOME = "${MAVEN_HOME}/bin"
        echo 'MAVEN_HOME--------->'+MAVEN_HOME
        //env.PATH = "${MAVEN_HOME}/bin:${env.PATH}"
        echo 'env.PATH----->['+ env.PATH +']'
        
        String mi = getMicroserviceInformation()
        echo 'MicroserviceInformation---------> '+mi
        
        try {
            runMavenVerify(MAVEN_HOME)
        } catch(Exception e) {
            echo 'Problem while building job:['+ e.getMessage() +']'
        }
        echo 'Ending the script...'
    }
    //Gatling Simulation script
    stage("Performance") {
        gatlingArchive()
    }
}

private String getMicroserviceInformation() {
   // def pom = readMavenPom file: 'pom.xml'
    
   // String artifactId = pom.artifactId
   // String groupId = pom.groupId
   // String version = pom.version

    String artifactId = "my-first-program"
    String groupId = "com.example.hello"
    String version = "1.0-SNAPSHOT"

    return artifactId+":"+groupId+":"+version
}

private String runMavenVerify(MAVEN_HOME) {
    // sh "ls -lrth ${MAVEN_HOME}" // executing shell commands in jenkinsfile
    // String chmodStatus = sh script: "chmod +x ${MAVEN_HOME}", returnStatus: true // executing shell commands in jenkinsfile and taking respose from that
    //echo chmodStatus
    //int verificationStatus = sh script: "${MAVEN_HOME}/mvn clean verify --fail-at-end --batch-mode --update-snapshots", returnStatus: true
    int verificationStatus = bat script: "${MAVEN_HOME}/mvn clean verify --fail-at-end --batch-mode --update-snapshots", returnStatus: true

    echo 'Verification Status:['+verificationStatus+']'
    
    if (verificationStatus != 0) {
        error('The Maven verification of the service has failed.')
    }
    else {
        echo 'Maven Stage Passed.'
    }
}

node('maven'){
    def mvnHome = tool name: 'maven360', type: 'maven'
    stage('Checkout'){
        echo "Downloading the source code.."
        git credentialsId: 'githubaccount', url: 'https://github.com/lokeshkamalay/simple-java-maven-app.git'
    }
    stage('Execute Test Cases'){
        echo "Executing Test Cases"
        sh "${mvnHome}/bin/mvn clean test"
        archiveArtifacts allowEmptyArchive: true, artifacts: 'target/surefire-reports/*'
        junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site/', reportFiles: 'surefire-report.html', reportName: 'HTMLReport', reportTitles: ''])
    }
    input 'Do you want to perform build?'
    stage('Build'){
        echo "Building the job now"
        sh "${mvnHome}/bin/mvn package -DskipTests=true"
    }
    stage('Post Build Actions'){
        echo "Sending an email to user"
    }
}


// Create Master
// Create Agent
// Configure Agent (in master, as a node)
// Setup Agent (Install jdk, maven) {wget, tar, alternatives}
// Manage Jenkins --> Global Tool Configuration (Maven installations)
//

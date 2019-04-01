node('maven'){
    def mvnHome = tool name: 'maven360', type: 'maven'
    stage('Checkout'){
        echo "Downloading the source code"
        git credentialsId: 'githubaccount', url: 'https://github.com/lokeshkamalay/simple-java-maven-app.git'
    }
    stage('Building maven'){
        sh "${mvnHome}/bin/mvn clean compile"
    }
    stage ('Packaging software'){
        sh "${mvnHome}/bin/mvn clean package"
    }
    stage ('testing junit case'){
        junit 'target/surefire-reports/*.xml'
    }   
    stage ('Archiving artifacts'){
        archiveArtifacts 'target/surefire-reports/*.xml' 
    }
    stage ('generating surfire report'){
        sh "${mvnHome}/bin/mvn surefire-report:report-only"
    }
    stage ('Publishing HTML report'){
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'HTML Report', reportTitles: ''])
    }
}

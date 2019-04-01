node('maven'){
    def mvnHome = tool name: 'maven360', type: 'maven'
    stage('Checkout'){
        echo "Downloading the source codes"
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
    stage ('Sending jar file to AWS s3 bucket'){
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-iam-key', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh "aws s3 cp target/my-app-1-RELEASE.jar s3://sushil-bh/"
    }
}

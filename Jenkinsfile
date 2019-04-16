<<<<<<< HEAD
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
    timeout(time: 30, unit: 'MINUTES') {
     input message: 'Do you want to Deploy?', ok: 'Deploy'
     sh "mutt -s 'The job is completed' sushilb126@gmail.com"
     }
    stage ('Sending jar file to AWS s3 bucket'){
    withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'aws-iam-key', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
    sh "aws s3 cp target/my-app-1-RELEASE.jar s3://sushil-bh/"
    }
    stage ('Deploying package to the target machine'){
        sshagent(['ssh-agent']) {
    sh "scp -o StrictHostKeyChecking=no target/my-app-1-RELEASE.jar ec2-user@34.205.30.165:/home/ec2-user"
}
    }
    }    
=======
node('mavenbuilds'){
    def mvnHome = tool name: 'maven354', type: 'maven'
    stage('checkout'){
        echo "Checking the Git code"
        git credentialsId: 'lokigithubapikey', url: 'https://github.com/lokeshkamalay/simple-java-maven-app.git'
    }
    stage('Executing Test Cases'){
        echo "Execuring Test Cases Started"
        sh "$mvnHome/bin/mvn clean test surefire-report:report-only"
        archiveArtifacts 'target/**/*'
        junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
        publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/site', reportFiles: 'surefire-report.html', reportName: 'SureFireReportHTML', reportTitles: ''])
        echo "Executing Test Cases Completed"
    }
    //stage('Sonar Analysis'){
    //    sh "$mvnHome/bin/mvn sonar:sonar -Dsonar.host.url=http://aefdc217.ngrok.io -Dsonar.login=d08d80d05ae55ae9de4ca22bc2fd5140c1308ee2"
    //}
    stage('Packaging'){
        echo "Preparing artifacts"
        sh "$mvnHome/bin/mvn package -DskipTests=true"
    }
    stage('Push to artifactory'){
          sh "$mvnHome/bin/mvn deploy -DskipTests=true --settings settings.xml"
    }
    stage('Deployments'){
        sh 'curl http://fa1b7800.ngrok.io/artifactory/maven-local/com/mycompany/app/my-app/1-RELEASE/my-app-1-RELEASE.jar -o my-app.jar'
        sshagent(['deployment-id']) {
            sh 'scp -o StrictHostKeyChecking=no my-app.jar ubuntu@172.31.94.69:~/'
        }

    }
>>>>>>> 2f38b7948be032cb230d840994859aa800121756
}

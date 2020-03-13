pipeline {
agent any

stages {
           stage('SCM Checkout'){
                steps{
                      git branch: 'Development', url: 'https://github.com/arunimaU/parking_frontend.git'
                }
            }
            stage('Build ui') {
                steps{
                    sh 'npm install'
                    sh 'npm run build'
                    sh 'cp -r $WORKSPACE/build /opt/apache-tomcat-9.0.31/webapps'
                    sh 'curl -u admin:admin http://13.127.197.181:8888/manager/reload?path=/build'
                }
            }

stage('Build Backend') {
steps {
        git branch: 'development', url: 'https://github.com/arunimaU/parking_backend.git'
         sh"/opt/apache-maven-3.6.3/bin/mvn clean package -Dmaven.test.skip=true "
}
}
 stage('Email notification'){

 steps{

 slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'slackjenkins', color: 'bad', message: "${env.BUILD_URL}", tokenCredentialId: 'arunimaslack', username: 'arunimauniyal'

 }

 }
 stage('Please Provide Approval for UAT Deployment ?'){
 steps{
 script{
  def userInput
  try {
    userInput = input(

        id: 'Proceed1', message: 'SIT Deployment Approval', parameters: [

        [$class: 'BooleanParameterDefinition', defaultValue: true, description: '', name: 'Please Confirm you agree with this']

        ])

} catch(err) {

    def user = err.getCauses()[0].getUser()

    userInput = false

    echo "Aborted by: [${user}]"

}

          }

        }

        }

          
          stage('Deployment-UAT'){

            steps{
                       sh"/opt/apache-maven-3.6.3/bin/mvn clean deploy -Dmaven.test.skip=true "
                    sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
                sshPublisher(publishers: [sshPublisherDesc(configName: 'server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''ansible-playbook backend_input1.yml
''', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])       }

          }
}
    
}


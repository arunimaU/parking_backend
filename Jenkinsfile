pipeline {
agent any

stages {
           stage('Git Checkout'){
                steps{
                    git 'https://github.com/arunimaU/parking_frontend.git'
                }
            }
            stage('Build') {
                steps{
                    sh 'npm install'
                    sh 'npm run build'
   
                }
            }
            stage('Deploy'){
                steps{
                    sh 'cp -r $WORKSPACE/build /opt/apache-tomcat-9.0.31/webapps'
                    sh 'curl -u admin:admin http://13.127.197.181:8888/manager/reload?path=/build'
                }
            }
         
         
         
         
         
         
         
         
         
/**Insurance-Backend Pipeline Job Build and Test stages **/
stage('SCM Checkout') {
steps {
git url: git branch: 'development', https://github.com/arunimaU/parking_backend.git'
}
}
stage('Build') {
steps {
         sh"/opt/apache-maven-3.6.3/bin/mvn clean package -Dmaven.test.skip=true "
}
}
 stage('SIT Approval'){

 steps{

 slackSend baseUrl: 'https://hooks.slack.com/services/', channel: 'slackjenkins', color: 'bad', message: "${env.BUILD_URL}", tokenCredentialId: 'arunimaslack', username: 'arunimauniyal'

 }

 }

        stage('Please Provide Approval for SIT Deployment ?'){

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

          

stage('Deploy') {
steps {
                     sh"/opt/apache-maven-3.6.3/bin/mvn clean deploy -Dmaven.test.skip=true "
}
}
stage('Release') {
steps {
                    sh"export JENKINS_NODE_COOKIE=dontKillMe; nohup java -jar $WORKSPACE/target/*.jar &"
}
}

          stage('Deployment-SIT'){

            steps{

                sshPublisher(publishers: [sshPublisherDesc(configName: 'server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook  backend_input1.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])

            }

          }





}
    
}

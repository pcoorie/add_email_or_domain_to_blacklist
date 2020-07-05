#!groovy

ansiColor('gnome-terminal') {
  echo "TERM=${env.TERM}"
  // prints out TERM=xterm
}
pipeline {
    agent any


    stages {
        stage('Getting things ready') {
            steps {
                echo 'Building..'
            }
        }
        stage('Adding emails and domains') {
            steps {
                echo 'Adding domain..'
                echo "You are about to block ${params.DOMAIN} from ${params.ACCOUNT} "
            }
        }
        stage('Permanently blocking.. ') {
            steps {
                echo 'Deploying....'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'mail', transfers: [sshTransfer(excludes: '', execCommand: "/opt/iRedAPD-4.1/tools/wblist_admin.py --account ${params.ACCOUNT} --add --blacklist ${params.DOMAIN}", execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
    post {
        always {
            echo 'Just for the record..'
            
            emailext body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider']],
                subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
                
        }
    }
}

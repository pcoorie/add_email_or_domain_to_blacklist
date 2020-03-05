#!groovy

ansiColor('xterm') {
  echo "TERM=${env.TERM}"
  // prints out TERM=xterm
}
pipeline {
    agent any


    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Adding email or domain') {
            steps {
                echo 'Adding domain..'
                echo "You are about to block ${params.DOMAIN} from ${params.ACCOUNT} "
            }
        }
        stage('Blocking with power') {
            steps {
                echo 'Deploying....'
                sshPublisher(publishers: [sshPublisherDesc(configName: 'mail', transfers: [sshTransfer(excludes: '', execCommand: "python /opt/iRedAPD-2.4/tools/wblist_admin.py --account ${params.ACCOUNT} --add --blacklist ${params.DOMAIN}", execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
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

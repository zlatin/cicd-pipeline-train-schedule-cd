pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Deploy to staging') {
            steps {
                sshPublisher(
                    publishers:
                    [sshPublisherDesc(
                        configName: 'staging',
                        transfers: [
                            sshTransfer(
                                excludes: '',
                                execCommand: 'systemctl start train-schedule; systemctl stop train-schedule',
                                execTimeout: 120000,
                                flatten: false,
                                makeEmptyDirs: false,
                                noDefaultExcludes: false,
                                patternSeparator: '[, ]+',
                                remoteDirectory: '/opt/train-schedule',
                                remoteDirectorySDF: false,
                                removePrefix: 'dist/',
                                sourceFiles: 'dist/trainSchedule.zip')],
                                usePromotionTimestamp: false,
                                useWorkspaceInPromotion: false,
                                verbose: false)])
            }
        }
    }
}

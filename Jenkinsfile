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
            when{
                branch 'master'
            }
            steps {
                sshPublisher(
                    publishers:
                    [
                        sshPublisherDesc(
                        configName: 'staging',
                        transfers: [
                            sshTransfer(
                                excludes: '',
                                execCommand: 'sudo systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo systemctl start train-schedule',
                                execTimeout: 120000,
                                flatten: false,
                                makeEmptyDirs: false,
                                noDefaultExcludes: false,
                                patternSeparator: '[, ]+',
                                remoteDirectory: '/tmp/',
                                remoteDirectorySDF: false,
                                removePrefix: 'dist/',
                                sourceFiles: 'dist/trainSchedule.zip'
                                )
                                ],
                                usePromotionTimestamp: false,
                                useWorkspaceInPromotion: false,
                                verbose: false
                                )
                                ]
                                )
            }
        }
        stage('Deploy to production') {
            when{
                branch 'master'
            }
            steps {
                input(message: 'Does staging look good?')
                milestone(1)
                sshPublisher(
                    publishers:
                    [
                        sshPublisherDesc(
                        configName: 'production',
                        transfers: [
                            sshTransfer(
                                excludes: '',
                                execCommand: 'sudo systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo systemctl start train-schedule',
                                execTimeout: 120000,
                                flatten: false,
                                makeEmptyDirs: false,
                                noDefaultExcludes: false,
                                patternSeparator: '[, ]+',
                                remoteDirectory: '/tmp/',
                                remoteDirectorySDF: false,
                                removePrefix: 'dist/',
                                sourceFiles: 'dist/trainSchedule.zip'
                                )
                                ],
                                usePromotionTimestamp: false,
                                useWorkspaceInPromotion: false,
                                verbose: false
                                )
                                ]
                                )
            }
        }
    }
}

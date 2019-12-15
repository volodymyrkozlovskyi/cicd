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
        stage('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                    sshPublisher(
                        publishers: [
                        sshPublisherDesc(
                            configName: 'staging',
                            transfers: [sshTransfer(
                                cleanRemote: false,
                                sourceFiles: 'dist/trainSchedule.zip\'',
                                removePrefix: 'dist/',
                                remoteDirectory: '/tmp',
                                execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule\''
                                )
                            ],
                            )
                        ]
                    )
                } 
            }
        }
    }

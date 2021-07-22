pipeline {
    agent any
    stages{
        stage("build") {
            steps {
                echo 'running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage("deploy to staging server") {
            when {
                branch 'master'
            }
            steps {
                withcredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')]) {
                    sshpublisher(
                        failOnError: true,
                        continueOnError: false,
                        publisher: [
                            sshpublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$USERPASS"
                                ],
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                        execCommand: 'sudo usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo usr/bin/systemctl start train-schedule'
                                    )
                                ]
                            )
                        ]
                    )
                }
            }
        }

    }
}

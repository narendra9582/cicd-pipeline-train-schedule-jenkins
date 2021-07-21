pipeline {
    agent any
    stages{
        stage("build") {
            steps {
                echo 'running build automation'
                sh './gradlew build --no-deamon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }

    }
}

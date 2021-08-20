pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                bat 'make' 
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true 
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                 /* `make check` returns non-zero on test failures,
                * using `true` to allow the Pipeline to continue nonetheless
                */
                bat 'make check || true' 
                junit '**/target/*.xml'
            }
        }
        stage('Deploy') {
            echo 'Deploying..'
          when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
          }
            steps {
                bat 'make publish'
            }
        }
    }
}

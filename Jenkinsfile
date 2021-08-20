pipeline {
    agent any
tools
    {
     maven 'Maven'   
    }
    stages {
        stage('Build') {
             when {
              expression {
                BRANCH_NAME == master || BRANCH_NAME == MFC520 
              }
          }
            steps {
                echo 'Building..'
                bat "mvn install"
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
          when {
              expression {
                currentBuild.result == null || currentBuild.result == 'SUCCESS' 
              }
          }
            echo 'Deploying..'
            steps {
                bat 'make publish'
            }
        }
    }
}


pipeline {
  agent { docker 'maven:3.3.9-jdk-8-onbuild-alpine' }
  stages {
    stage('build') {
      steps {
        sh 'mvn clean package'
      }
      post {
        success {
            archive "target/**/*"
            }
        }
    }

    stage('integration test') {
        steps {
            sh 'mvn verify'
        }
        post {
         always {
            junit '**/target/surefire-reports/TEST-*.xml'
          }
        }
    }

    stage('quality and functional tests') {
        steps {
            parallel (
                "Firefox" : {
                    sh "echo testing FFX"
                    sh "echo more steps"
                },
                "Chrome" : {
                    sh "echo testing Chrome"
                    sh "echo more steps"
                }
            )
        }
    }

    stage('Approval') {
        when { branch "master" }

        steps {
            input 'Deploy to Production?'
        }
    }

    stage('Production') {
        when { branch "master" }
        steps {
            sh "echo 'deploy to prod'"
        }
    }

  }
}

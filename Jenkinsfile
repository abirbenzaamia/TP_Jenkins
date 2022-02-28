pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      steps {
        sh '${gradle} build'
        sh '${gradle} javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/javadoc/**'
        junit 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build status', body: 'The build was successful', to: 'ia_benzaamia@esi.dz', replyTo: 'ia_benzaamia@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            withSonarQubeEnv('sonar') {
              sh './gradlew sonarqube'
            }

            waitForQualityGate true
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber(fileIncludePattern: '*/.json', jsonReportDirectory: 'reports/')
          }
        }

      }
    }

    stage('Deployment') {
      steps {
        sh '${gradle} publish'
      }
    }

  }
}
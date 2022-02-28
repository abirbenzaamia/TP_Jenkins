pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      steps {
        sh 'java -version'
        sh './gradlew build'
        sh './gradlew javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/javadoc/**'
        junit 'build/test-results/test/*.xml'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build succeed ', body: 'Hello , The build finished with sucess', to: 'ia_benzaamia@esi.dz', replyTo: 'ia_benzaamia@esi.dz')
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
            cucumber(fileIncludePattern: 'build/reports/cucumber', jsonReportDirectory: 'reports/', reportTitle: 'example-report.json')
          }
        }

      }
    }

  }
}
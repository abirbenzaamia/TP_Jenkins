pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      environment {
        gradle = '/usr/local/bin/gradle'
      }
      steps {
        sh './gradlew build'
        sh './gradlew javadoc'
        archiveArtifacts 'build/libs/**/*.jar'
        archiveArtifacts 'build/docs/javadoc/**'
        junit 'build/test-results/test/*.xml'
        catchError(buildResult: 'failure', stageResult: 'post') {
          mail(subject: 'Build status', body: 'The build failed!', to: 'ia_benzaamia@esi.dz')
        }

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
            sh '${gradle} sonarqube'
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
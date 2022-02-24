pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      environment {
        gradle = '/usr/local/bin/gradle'
      }
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
        mail(subject: 'Build succeed ', body: 'Hello , The build finished with sucess', to: 'ia_benzaamia@esi.dz', replyTo: 'ia_benzaamia@esi.dz')
      }
    }

    stage('Code Analysis') {
      parallel {
        stage('Code Analysis') {
          steps {
            sh '${gradle} sonarqube'
          }
        }

        stage('Test Reporting') {
          steps {
            cucumber 'build/jacoco/.exec'
          }
        }

      }
    }

  }
}
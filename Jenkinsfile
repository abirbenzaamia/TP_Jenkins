pipeline {
  agent any
  stages {
    stage('Build') {
      agent any
      environment {
        gradle = '/usr/local/bin/gradle'
      }
      steps {
        sh '/usr/local/bin/gradle build'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build succeed ', body: 'Hello , The build finished with sucess', to: 'ia_benzaamia@esi.dz')
      }
    }

  }
}
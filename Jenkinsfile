pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'gradle build'
      }
    }

    stage('Mail Notification') {
      steps {
        mail(subject: 'Build succeed ', body: 'Hello , The build finished with sucess', to: 'ia_benzaamia@esi.dz')
      }
    }

  }
}
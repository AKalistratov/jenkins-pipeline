pipeline {
  agent { dockerfile true }
  stages {
    stage('Test') {
      steps {
        sh '''
          git --version
          curl --version
          cmake --version
        '''
      }
    }
  }
}
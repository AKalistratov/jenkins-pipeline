pipeline {
  agent { dockerfile true }
  stages {
    stage('Git checkout') {
      steps {
        git branch: 'main', credentialsId: 'ilyataskaev', url: 'https://github.com/SWTec/cppinternship21-phase1.git'
      }
    }
    stage('Test') {
      steps {
       sh '''
        cd json_project
        cmake .
        cmake --build .
        pip --version
        cpplint --version
        cpplint --extensions=h,hpp,c,cpp,cc,cu,hh,ipp --recursive --filter="-legal/copyright,-readability/braces,-whitespace/braces,-whitespace/comments,-whitespace/indent,-whitespace/newline,-whitespace/operators,-whitespace/parens,-whitespace/tab,-whitespace/line_length" --exclude=3rdparty/* --output=junit ./ 2> cpplint.xml || true
        ls cpplint.xml
        '''
      }
    }
  }
}



pipeline {
  agent { dockerfile true }
  stages {
    stage('Git checkout') {
      steps {
        git branch: 'main', credentialsId: 'ilyataskaev', url: 'https://github.com/SWTec/cppinternship21-phase1.git'
      }
    }
    stage('Build') {
      steps {
       sh '''
        cd json_project
        cmake .
        cmake --build .
        cpplint --extensions=h,hpp,c,cpp,cc,cu,hh,ipp --recursive --filter="-legal/copyright,-readability/braces,-whitespace/braces,-whitespace/comments,-whitespace/indent,-whitespace/newline,-whitespace/operators,-whitespace/parens,-whitespace/tab,-whitespace/line_length" --exclude=3rdparty/* --output=junit ./ 2> cpplint.xml || true
        '''
      }
    }
    stage('Publish result') {
      steps {
       junit allowEmptyResults: true, skipMarkingBuildUnstable: true, testResults: 'json_project/cpplint.xml'
       archiveArtifacts artifacts: 'json_project/hello_test, json_project/JsonDesLib', followSymlinks: false
      }
    }
  }
}


#
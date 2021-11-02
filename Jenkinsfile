pipeline {
  agent { dockerfile true }
  parameters {
        string(name: 'BUILD_BRANCH', defaultValue: 'main', description: 'You can choose another branch to build.')
  }
  environment {
        GH_TOKEN = credentials('GH_TOKEN')
  }
  stages {
    stage('Git checkout') {
      steps {
        sh "pwd; printenv; ls -al"
        sh "git clone https://oauth2:${GH_TOKEN}@github.com/SWTec/cppinternship21-phase1.git"
      }
    }
    stage('Build') {
      steps {
       sh '''
        cd cppinternship21-phase1/json_project
        cmake .
        cmake --build .
        cpplint --extensions=h,hpp,c,cpp,cc,cu,hh,ipp \
        --recursive \
        --filter="-legal/copyright,\
        -readability/braces,\
        -whitespace/braces,\
        -whitespace/comments,\
        -whitespace/indent,\
        -whitespace/newline,\
        -whitespace/operators,\
        -whitespace/parens,\
        -whitespace/tab,\
        -whitespace/line_length" \
        --exclude=3rdparty/* \
        --output=junit ./ 2> cpplint.xml || true
        '''
      }
    }
  }
  post {
      success {
        junit allowEmptyResults: true, skipMarkingBuildUnstable: true, testResults: 'cppinternship21-phase1/json_project/cpplint.xml'
        archiveArtifacts artifacts: 'cppinternship21-phase1/json_project/hello_test, cppinternship21-phase1/json_project/JsonDesLib', followSymlinks: false
      }
      cleanup {
        cleanWs()
      }
  }
}
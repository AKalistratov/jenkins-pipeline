pipeline {
  agent { dockerfile true }
  parameters {
        string(name: 'BUILD_BRANCH', defaultValue: 'main', description: 'You can choose another branch to build.')
  }
  stages {
    stage('Git checkout') {
      steps {
        git branch: "${BUILD_BRANCH}", credentialsId: 'ilyataskaev', url: 'https://github.com/SWTec/cppinternship21-phase1.git'
      }
    }
    stage('Build') {
      steps {
       sh '''
        cd json_project
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
        junit allowEmptyResults: true, skipMarkingBuildUnstable: true, testResults: 'json_project/cpplint.xml'
        archiveArtifacts artifacts: 'json_project/hello_test, json_project/JsonDesLib', followSymlinks: false
      }
  }
}
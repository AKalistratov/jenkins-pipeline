pipeline {
  agent { dockerfile true }
  parameters {
        string(name: 'BUILD_BRANCH', defaultValue: 'main', description: 'You can choose another branch to build.')
  }
  environment {
        GH_TOKEN = credentials('GH_TOKEN')
        ORG_NAME = "SWTec"
        REPO_NAME = "cppinternship21-phase1"
  }
  stages {
    stage('Git checkout') {
      steps {
        sh "pwd; printenv; ls -al"
        sh "git clone -b ${BUILD_BRANCH} https://oauth2:${GH_TOKEN}@github.com/${ORG_NAME}/${REPO_NAME}.git"
      }
    }
    stage('Build') {
      steps {
       sh """
        cd ${REPO_NAME}/json_project
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
        """
      }
    }
  }
  post {
      success {
        junit allowEmptyResults: true, skipMarkingBuildUnstable: true, testResults: "${REPO_NAME}/json_project/cpplint.xml"
        archiveArtifacts artifacts: "${REPO_NAME}/json_project/hello_test, ${REPO_NAME}/json_project/JsonDesLib", followSymlinks: false
      }
      cleanup {
        cleanWs()
      }
  }
}
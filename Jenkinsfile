node {
  stage('Build') {
    checkout scm // memastikan jenkins melakukan fetch/pull code terlebih dahulu

    docker.image('python:2-alpine').inside() {
      sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    }
  }
  stage('Test') {
    docker.image('qnib/pytest').inside() {
      try {
        sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
      }
      finally {
        junit 'test-reports/results.xml'
      }
    }
  }

  stage('Manual Approval') {
    //input message: 'Lanjutkan ke tahap Deploy? (Tekan tombol "Proceed" untuk melanjutkan)'
    docker.image('cdrx/pyinstaller-linux:python2').inside {
      sh 'pyinstaller --help'
      //sh 'pyinstaller --onefile sources/add2vals.py'
    }
  }

  stage('Deploy') {
    docker.image('cdrx/pyinstaller-linux:python2').inside {
      sh 'pyinstaller --help'
      //sh 'pyinstaller --onefile sources/add2vals.py'
    }
    //try {
    //docker.image('cdrx/pyinstaller-linux:python2').inside() {
    //}
    //}
    //finally {
    //  archiveArtifacts 'dist/add2vals'
    //}
  }
    // TODO: Run python
}

//pipeline {
//    agent none
//    stages {
//        stage('Build') {
//            agent {
//                docker {
//                    image 'python:2-alpine'
//                }
//            }
//            steps {
//                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
//            }
//        }
//        stage('Test') {
//            agent {
//                docker {
//                    image 'qnib/pytest'
//                }
//            }
//            steps {
//                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
//            }
//            post {
//                always {
//                    junit 'test-reports/results.xml'
//                }
//            }
//        }
//        stage('Deliver') {
//            agent {
//                docker {
//                    image 'cdrx/pyinstaller-linux:python2'
//                }
//            }
//            steps {
//                sh 'pyinstaller --onefile sources/add2vals.py'
//            }
//            post {
//                success {
//                    archiveArtifacts 'dist/add2vals'
//                }
//            }
//        }
//    }
//}

node { 
  stage('Build') {
    checkout scm // memastikan jenkins melakukan fetch/pull code terlebih dahulu

    //docker.image('python:2-alpine').inside() {
    //  sh 'python -m py_compile sources/add2vals.py sources/calc.py'
    //}
  }
  //stage('Test') {
  //  docker.image('qnib/pytest').inside() {
  //    try {
  //      sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
  //    }
  //    finally {
  //      junit 'test-reports/results.xml'
  //    }
  //  }
  //}

  //stage('Manual Approval') {
  //  input message: 'Lanjutkan ke tahap Deploy? (Tekan tombol "Proceed" untuk melanjutkan)'
  //}

  stage('Deploy') {
    sh 'docker run --rm -v ".:/src/" cdrx/pyinstaller-linux:python2 -c "pyinstaller --help"'
    //sh 'docker run -v "$(pwd):/src/" cdrx/pyinstaller-linux:python2 -c "pyinstaller --onefile sources/add2vals.py'
    //docker.image('cdrx/pyinstaller-linux:python2').inside {
    //  sh 'pyinstaller --help'
    //  //sh 'pyinstaller --onefile sources/add2vals.py'
    //}
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

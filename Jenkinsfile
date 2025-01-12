node { 

  def remote = [:]
  remote.name = "aws-server"
  remote.allowAnyHosts = true

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
    input message: 'Lanjutkan ke tahap Deploy? (Tekan tombol "Proceed" untuk melanjutkan)'
  }

  stage('Deploy') {
    sh 'docker run --rm -v ".:/src/" cdrx/pyinstaller-linux:python2 -c "pyinstaller --onefile sources/add2vals.py"'
    
    // Simpan hasil build ke artifact
    archiveArtifacts 'dist/add2vals'

    withCredentials([sshUserPrivateKey(credentialsId: 'aws-server', keyFileVariable: 'identity',  usernameVariable: 'userName'), string(credentialsId: 'server-ip', variable: 'ip')]) {
      remote.host = ip 
      remote.user = userName
      remote.identityFile = identity

      sshPut remote: remote, from: 'dist/add2vals', into: '.'
      sshCommand remote: remote, command: 'ls'
      sshCommand remote: remote, command: 'add2vals 10 10'
    }

    // hapus directory hasil build
    sh 'docker run --rm -v ".:/src/" cdrx/pyinstaller-linux:python2 -c "rm -rf build dist"'
  }
}

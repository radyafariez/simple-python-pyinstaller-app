node {
    // 
    withDockerContainer('python:2-alpine') {
        // 
        stage('Build') {
            checkout scm
            sh 'python -m py_compile sources/add2vals.py sources/calc.py'
        }
    }
    withDockerContainer('qnib/pytest') {
        // 
        stage('Test') {
            checkout scm
            sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
            junit 'test-reports/results.xml'
        }
    }
    withDockerContainer('cdrx/pyinstaller-linux:python2')
    stage('Deploy') {
        // 
        input message: 'Lanjutkan ke tahap Deploy?'
        checkout scm
        sh 'pyinstaller --onefile sources/add2vals.py'
        sleep time: 1, unit: 'MINUTES'
    }
    post {
        success {
            archiveArtifacts 'dist/add2vals'
            }
        }
}

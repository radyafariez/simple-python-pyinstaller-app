pipeline {
    agent any
    
    stages {
        stage('Build') {
            steps {
                script {
                    // Menggunakan Docker container dengan image 'python:2-alpine'
                    docker.image('python:2-alpine').inside {
                        // Checkout kode dari SCM
                        checkout scm
                        // Compile Python source code
                        sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                    }
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    // Menggunakan Docker container dengan image 'qnib/pytest'
                    docker.image('qnib/pytest').inside {
                        // Checkout kode dari SCM
                        checkout scm
                        // Menjalankan pengujian dengan Pytest
                        sh 'py.test --junit-xml test-reports/results.xml sources/test_calc.py'
                        // Menghasilkan laporan pengujian JUnit
                        junit 'test-reports/results.xml'
                    }
                }
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    // Menggunakan Docker container dengan image 'cdrx/pyinstaller-linux:python2'
                    docker.image('cdrx/pyinstaller-linux:python2').inside {
                        // Input message untuk tahap Deploy
                        input message: 'Lanjutkan ke tahap Deploy?'
                        // Checkout kode dari SCM
                        checkout scm
                        // Mengkompilasi Python source code menjadi satu file executable
                        sh 'pyinstaller --onefile sources/add2vals.py'
                        // Tunggu selama 1 menit sebelum melanjutkan
                        sleep time: 1, unit: 'MINUTES'
                    }
                }
            }
        }
    }
    
    post {
        success {
            // Menyimpan artifact hasil deploy
            archiveArtifacts 'dist/add2vals'
        }
    }
}

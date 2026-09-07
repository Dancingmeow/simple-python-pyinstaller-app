pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
        timestamp()
    }
    stages {
        stage('Setup') {
            steps {
                powershell '''
                    uv --version
                    uv sync --locked
                '''
            }
        }
        stage('Build') {
            steps {
                powershell 'uv run --locked python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            steps {
                powershell 'uv run --locked python -m pytest --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                powershell 'uv run --locked python -m pyinstaller --onefile sources/add2vals.py'
            }
            post {
                success {
                    archiveArtifacts 'dist/add2vals'
                }
            }
        }
    }
}

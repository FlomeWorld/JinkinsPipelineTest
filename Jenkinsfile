pipeline {
    agent { docker { image 'golang' } }
    stages {
        stage('build') {
            steps {
                sh 'go version'
            }
        }
        stage('deploy') {
            steps {
                retry(3) {
                    chmod +777 ./hack/flakey-deploy.sh
                    sh './hack/flakey-deploy.sh'
                }
                timeout(time: 3, unit: 'MINUTES') {
                    chmod +777 ./hack/health-check.sh
                    sh './hack/health-check.sh'
                }
            }
        }
    }
    post {
            always {
                echo 'This will always run'
            }
            success {
                echo 'This will run only if successful'
            }
            failure {
                echo 'This will run only if failed'
            }
            unstable {
                echo 'This will run only if the run was marked as unstable'
            }
            changed {
                echo 'This will run only if the state of the Pipeline has changed'
                echo 'For example, if the Pipeline was previously failing but is now successful'
            }
        }
}
pipeline {
    agent any
    stages {
            stage('Build') {
                steps {
                    sh 'echo "Hello World"'
                    sh '''
                        echo "Multiline shell steps works too"
                        ls -lah
                    '''
                }
            }
            stage('Deploy') {
                steps {
                    timeout(time: 3, unit:'MINUTES'){
                        retry(5) {
                            sh './flakey-deploy.sh'
                        }
                    }
                }
            }
            stage('Test') {
                steps {
                    sh 'echo "Fail!"; exit 1'
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
    }
}
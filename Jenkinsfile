pipeline {
    agent {
        kubernetes {
            yaml '''
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: shell
    image: node:6-alpine
    command:
    - cat
    tty: true
'''
            defaultContainer 'shell'
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
            steps {
                sh 'npm build' 
            }
            steps {
                sh '''
                pwd
                ls -al
                '''
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}

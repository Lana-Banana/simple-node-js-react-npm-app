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
        stage('Npm install') { 
            steps {
                sh '''
                  npm install
                  pwd
                  ls -al
                '''
            }
        }
        stage('Build') { 
            steps {
                sh '''
                  echo 'The following "npm" command builds your Node.js/React application for'
                  echo 'production in the local "build" directory (i.e. within the'
                  echo '"/var/jenkins_home/workspace/simple-node-js-react-app" directory),'
                  echo 'correctly bundles React in production mode and optimizes the build for'
                  echo 'the best performance.'
                  set -x
                  npm run build
                  set +x
                '''
                sh '''
                  pwd
                  ls -al
                '''
            }
        }
        stage('Deliver') {
            steps {
                sh '''
                  echo 'The following "npm" command runs your Node.js/React application in'
                  echo 'development mode and makes the application available for web browsing.'
                  echo 'The "npm start" command has a trailing ampersand so that the command runs'
                  echo 'as a background process (i.e. asynchronously). Otherwise, this command'
                  echo 'can pause running builds of CI/CD applications indefinitely. "npm start"'
                  echo 'is followed by another command that retrieves the process ID (PID) value'
                  echo 'of the previously run process (i.e. "npm start") and writes this value to'
                  echo 'the file ".pidfile".'
                  set -x
                  npm start &
                  sleep 1
                  echo $! > .pidfile
                  set +x

                  echo 'Now...'
                  echo 'Visit http://localhost:3000 to see your Node.js/React application in action.'
                  echo '(This is why you specified the "args ''-p 3000:3000''" parameter when you'
                  echo 'created your initial Pipeline as a Jenkinsfile.)'
                '''
            }
        }
        stage('Input') {
            steps {
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
            }
        }
        stage('Kill') {
            steps {
                sh '''
                  echo 'The following command terminates the "npm start" process using its PID'
                  echo '(written to ".pidfile"), all of which were conducted when "deliver.sh"'
                  echo 'was executed.'
                  set -x
                  kill $(cat .pidfile)
                '''
            }
        }
    }
}

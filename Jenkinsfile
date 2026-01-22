pipeline {
    agent {
        kubernetes {
            // This YAML defines the "Docker Container" you want to use
            yaml '''
            apiVersion: v1
            kind: Pod
            spec:
                containers:
                    - name: my-builder  # We will refer to this name later
                        image: node:18-alpine
                        command:
                            - cat
                        tty: true
            '''
        }
    }
    stages {
        stage('Test npm') {
            steps {
                container('my-builder') {
                    sh 'npm --version'
                    sh 'node --version'
                }
            }
        }
        stage('Build') {
            steps {
                container('my-builder') {
                    sh 'npm ci'
                    sh 'npm run build'
                }
            }
        }
        stage('Test Build') {
            steps {
                container('my-builder') {
                    sh 'npm run test'
                }
            }
        }
    }
}

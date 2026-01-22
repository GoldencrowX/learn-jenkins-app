pipeline {
    agent any
    stages {
        stage('Test npm') {
            agent {
                docker {
                    image 'node:18-alpine' // ใช้ Image นี้ที่มี Node.js มาให้แล้ว
                    reuseNode true
                }
            }
            steps {
                sh 'npm --version'
                sh 'node --version'
            }
        }
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine' // ใช้ Image นี้ที่มี Node.js มาให้แล้ว
                    reuseNode true
                }
            }
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }
    }
}

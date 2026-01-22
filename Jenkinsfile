pipeline {
  environment {
    VERCEL_PROJECT_NAME = 'learn-jenkins-app'
    VERCEL_TOKEN = credentials('vercel-token') // ดึงจาก Jenkins
  }
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
    stage('Deploy') {
      steps {
        // ติดตั้ง Vercel CLI แบบ local
        sh 'npm install vercel -g'
        // สั่ง Deploy ไปยัง Vercel 
        // ใช้ --prebuilt เพื่อระบุว่าไฟล์ในโฟลเดอร์ build/ ได้ถูกสร้างไว้แล้ว
        sh './node_modules/.bin/vercel deploy --prod --prebuilt'
      }
    }

  }
  post {
    always {
      junit 'test-results/junit.xml'
    }
  }
}

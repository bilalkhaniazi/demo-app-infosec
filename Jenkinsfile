pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        echo 'Build demo-app'
        sh 'sh run_build_script.sh'
      }
    }
    stage('Scan') {
        snykSecurity organisation: 'prashant.b', projectName: 'nodejs_demo_snyk', severity: 'medium', snykInstallation: 'Snyk', snykTokenId: '87cd2da3-ccfa-46f7-b7d4-d115b400422c', targetFile: 'package.json'
    }

    stage('Linux Tests') {
      parallel {
        stage('Linux Tests') {
          steps {
            echo 'Run Linux tests'
            sh 'sh run_linux_tests.sh'
          }
        }

        stage('Windows Tests') {
          steps {
            echo 'Run Windows tests'
          }
        }

      }
    }

    stage('Deploy Staging') {
      steps {
        echo 'Deploy to staging environment'
        input 'Ok to deploy to production?'
      }
    }

    stage('Deploy Production') {
      post {
        always {
          archiveArtifacts(artifacts: 'target/demoapp.jar', fingerprint: true)
        }

        failure {
          emailext(subject: 'Demoapp build failure', to: 'bvazcosta@gmail.com', body: 'Build failure for demoapp Build ${env.JOB_NAME} ')
        }

      }
      steps {
        echo 'Deploy to Prod'
      }
    }

  }
}

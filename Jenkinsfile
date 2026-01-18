
pipeline {
  agent any

  options {
    // Helpful logs and timeouts
    timestamps()
    ansiColor('xterm')
    timeout(time: 15, unit: 'MINUTES')
  }

  stages {
    stage('Checkout') {
      steps {
        // If this is a Multibranch Pipeline job, Jenkins will do SCM checkout automatically.
        // If it's a simple Pipeline job, keep the checkout block below:
        checkout([$class: 'GitSCM',
          branches: [[name: '*/main']],   // change to your branch if not main
          userRemoteConfigs: [[url: 'https://github.com/pravallikathaniru/running_script.git']]
        ])
      }
    }

    stage('Verify workspace') {
      steps {
        sh '''
          echo "PWD: $(pwd)"
          echo "Listing root:"
          ls -la
          echo "Type of myip.sh:"
          file myip.sh || true
        '''
      }
    }

    stage('Normalize line endings') {
      steps {
        // Convert CRLF->LF in case the script was edited on Windows.
        sh '''
          if [ -f myip.sh ]; then
            sed -i 's/\\r$//' myip.sh
          else
            echo "ERROR: myip.sh not found in $(pwd)"
            exit 1
          fi
        '''
      }
    }

    stage('Make executable') {
      steps {
        sh '''
          chmod +x myip.sh
          ls -l myip.sh
          head -n1 myip.sh || true
        '''
      }
    }

    stage('Run script') {
      steps {
        // Use bash explicitly to support bash-only syntax
        sh '''
          set -euo pipefail
          echo "Running myip.sh with bash..."
          bash ./myip.sh
        '''
      }
    }
  }

  post {
    success {
      echo '✅ myip.sh executed successfully.'
    }
    failure {
      echo '❌ Build failed. Check the console log above.'
    }
    always {
      sh 'echo "Finished at: $(date)"'
    }
  }
}

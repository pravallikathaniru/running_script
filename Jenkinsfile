
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/pravallikathaniru/running_script.git'
            }
        }

        stage('Run Script') {
            steps {
                sh 'chmod +x myip.sh'
                sh './myip.sh'
            }
        }
    }
}

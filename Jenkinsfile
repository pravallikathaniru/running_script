
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run Script') {
            steps {
                sh '''
                    echo "Listing files..."
                    ls -la

                    # Fix line endings (important if file edited on Windows)
                    sed -i 's/\\r$//' myip.sh

                    # Make script executable
                    chmod +x myip.sh

                    echo "Running myip.sh..."
                    bash myip.sh
                '''
            }
        }
    }
}

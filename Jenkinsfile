pipeline {
    agent any

    environment {
        GO_VERSION = "1.22"
    }

    stages {
        stage('Setup Go') {
            steps {
                echo "Installing Go..."

                sh """
                if ! command -v go >/dev/null 2>&1; then
                    echo 'Go not installed. Installing...'
                    brew install go || true
                fi

                go version
                """
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Download Dependencies') {
            steps {
                sh """
                go mod tidy
                go mod download
                """
            }
        }

        stage('Build') {
            steps {
                sh """
                go build -o chromedp-app main.go
                """
            }
        }

        stage('Run Screenshot Script') {
            steps {
                sh """
                ./chromedp-app
                """
            }
        }

        stage('Archive Screenshot') {
            steps {
                archiveArtifacts artifacts: 'golang.png', fingerprint: true
            }
        }
    }
}


pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub'
                checkout scm
            }
        }

        stage('Verify Environment') {
            steps {
                sh '''
                    java -version
                    mvn -version
                    node -v
                    npm -v
                '''
            }
        }

        stage('Build Backend') {
            steps {
                dir('backend') {
                    echo 'Building Spring Boot backend'
                    sh 'mvn clean package'
                }
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    echo 'Installing Angular dependencies'
                    sh 'npm install'

                    echo 'Building Angular frontend'
                    sh 'npm run build'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts(
                    artifacts: 'backend/target/*.jar',
                    fingerprint: true
                )

                archiveArtifacts(
                    artifacts: 'frontend/dist/**/*',
                    allowEmptyArchive: true,
                    fingerprint: true
                )
            }
        }
    }

    post {
        success {
            echo 'CI PIPELINE COMPLETED SUCCESSFULLY!'
        }

        failure {
            echo 'CI PIPELINE FAILED! Check Console Output.'
        }
    }
}

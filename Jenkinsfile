pipeline {
    agent any

    stages {
        stage('Run Frontend') {
            steps {
                echo 'Installing frontend dependencies with Yarn...'
                // If frontend is in a subfolder, wrap with: dir('frontend') { ... }
                nodejs('Node-10.17') {
                    // In CI/CD, --frozen-lockfile ensures dependencies match yarn.lock exactly
                    sh 'yarn install --frozen-lockfile'
                }
            }
        }

        stage('Run Backend') {
            steps {
                echo 'Checking Gradle wrapper version...'
                // If backend is in a subfolder, wrap with: dir('backend') { ... }
                withGradle {
                    // Ensure the wrapper has execute permissions on Linux agents
                    sh 'chmod +x ./gradlew'
                    sh './gradlew -v'
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed. Check tool configurations (NodeJS / Gradle) or script permissions.'
        }
    }
}

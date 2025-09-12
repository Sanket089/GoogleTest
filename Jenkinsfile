pipeline {
    agent any

    tools {
        // Must match the names you configured in Jenkins → Global Tool Configuration
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Checking out code from GitHub...'
                git branch: 'main', 
                    url: 'https://github.com/Sanket089/GoogleTest.git', 
                    credentialsId: '3ab732b9-1099-442c-b68e-a79df6b6b9f0'
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project using Maven...'
                sh 'mvn clean compile'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running TestNG tests...'
                sh 'mvn test'
            }
            post {
                always {
                    echo 'Publishing TestNG/JUnit test results...'
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging the project (optional)...'
                sh 'mvn package'
            }
        }
        stage('Archive') {
    steps {
        echo 'Archiving artifacts...'
        archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
    }
}

    }

    post {
        success {
            echo 'Build and tests completed successfully!'
        }
        failure {
            echo 'Build or tests failed. Check console output for errors.'
        }
        always {
            echo 'Cleaning workspace...'
            cleanWs()
        }
    }
}

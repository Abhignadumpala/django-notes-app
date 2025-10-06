@Library('shared') _
pipeline {
    
    agent { label "vinod" }

    options {
        // Keep only the last 5 builds to save space
        buildDiscarder(logRotator(numToKeepStr: '5'))
        // Timestamps in console logs
        timestamps()
    }

    stages {

        // Clean workspace before starting
        stage('Cleanup Workspace') {
            steps {
                echo "Cleaning workspace..."
                deleteDir()  // Deletes all files in the workspace
            }
        }

        stage('Hello') {
            steps {
                script {
                    hello()
                }
            }
        }

        stage('Code') {
            steps {
                script {
                    clone("https://github.com/Abhignadumpala/django-notes-app.git", "main")
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    docker_build("notes-app", "latest", "abhignadumpala1")
                }
            }
        }

        stage('Push to DockerHub') {
            steps {
                script {
                    docker_push("notes-app", "latest", "abhignadumpala1")
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying the code..."
                sh "docker compose down && docker compose up -d"
            }
        }
    }

    post {
        always {
            echo "Cleaning workspace (excluding database volume)..."
        sh 'sudo rm -rf * || true'
        }
    }
}

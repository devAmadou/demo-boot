pipeline {
    agent any

    stages {
        stage('Connect to Github') {
            steps {
                // Get some code from a GitHub repository
                git(
                    url : "https://github.com/devAmadou/demo-boot.git",
                    branch :  "main"
                )
            }
        }

        stage('Build Artifact sans les Tests') {
            steps {
                sh 'mvn clean package -DskipTests=true'
            }
        }

        stage('Lancer les Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Image Docker') {
            steps {
                withDockerRegistry([credentialsId: "my-docker-hub", url: ""]) {
                    sh 'printenv'
                    sh 'docker build -t amkam009/demo-project:$BUILD_NUMBER .'
                    sh 'docker push amkam009/demo-project:$BUILD_NUMBER'
                }
            }
        }

        stage('Deploy to Dev') {
            when { expression { false } }
            steps {
                sh 'docker stop app-test-dockerisation || true'
                sh 'docker rm app-test-dockerisation || true'
                sh 'docker run -p 8180:8180 -d --name app-test-dockerisation amkam009/demo-project:$BUILD_NUMBER'
            }
        }
    }
}

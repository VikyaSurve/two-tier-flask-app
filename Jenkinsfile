pipeline {
    agent any;

    stages {
        stage("Code Clone") {
            steps {
                git url: "https://github.com/VikyaSurve/two-tier-flask-app.git", branch: "dev"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh ke dega..."
            }
        }

        stage("Push to Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId:"dockerhubcreds",
                    passwordVariable:"dockerhubPass",
                    usernameVariable:"dockerhubUser"
                    )])
                {
                    sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                    sh "docker image tag two-tier-flask-app ${env.dockerhubUser}/two-tier-flask-app"
                    sh "docker push ${env.dockerhubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
}

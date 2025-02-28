pipeline {
    agent { label "dev" }

    stages {
        stage("Code Clone") {
            steps {
                script {
                    echo "Cloning repository..."
                    git url: "https://github.com/Vikas-DevOpsPractice/two-tier-flask-app.git", branch: "dev"
                }
            }
        }

        stage("Test") {
            steps {
                script {
                    echo "Running tests..."
                }
            }
        }

        stage("Push to Docker Hub") {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: "dockerhubcreds",
                        passwordVariable: "dockerhubPass",
                        usernameVariable: "dockerhubUser"
                    )]) {
                        echo "Logging into Docker Hub..."
                        sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                        sh "docker image tag two-tier-flask-app ${env.dockerhubUser}/two-tier-flask-app"
                        sh "docker push ${env.dockerhubUser}/two-tier-flask-app:latest"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                script {
                    echo "Deploying application..."
                    sh "docker compose up -d --build flask-app" 
                }
            }
        }
    }

    post {
        success {
            script {
                echo "Build Succeeded. Sending Email..."
                emailext(
                    from: 'vikasmsurve@gmail.com',
                    to: 'vikasmsurve@gmail.com',
                    subject: 'Build Success: Demo CICD App',
                    body: '🎉 Build was successful for Demo CICD App! \n\n View Logs in Jenkins.'
                )
            }
        }
        
        failure {
            script {
                echo "Build Failed. Sending Failure Email..."
                emailext(
                    from: 'vikasmsurve@gmail.com',
                    to: 'vikasmsurve@gmail.com',
                    subject: '❌ Build Failed: Demo CICD App',
                    body: '🚨 Build failed for Demo CICD App. Check Jenkins for details!'
                )
            }
        }
    }
}

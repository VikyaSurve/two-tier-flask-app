pipeline{
    
    agent { label "dev"}

    environment {
        UID = sh(script: 'id -u', returnStdout: true).trim()
        GID = sh(script: 'id -g', returnStdout: true).trim()
    }

    stages{
        stage("Code Clone"){
            steps{
               script{
                    git url: "https://github.com/Vikas-DevOpsPractice/two-tier-flask-app.git", branch: "dev"
               }
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
                sh """
                docker compose pull
                docker compose up -d --build --force-recreate flask-app
                """
            }
        }
    }

    post{
        success{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'mentor@trainwithshubham.com',
                to: 'mentor@trainwithshubham.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}

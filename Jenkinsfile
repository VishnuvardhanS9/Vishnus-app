pipeline {
    agent { label "dev-server" }

    stages {
        stage("code clone") {
            steps {
                git url: "https://github.com/VishnuvardhanS9/Vishnus-app.git", branch: "main"
            }
        }

        stage("build") {
            steps {
                echo " code clone stage" 
                sh 'docker build -t vishnuapp .'
            }
        }

        stage("docker push") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPassword'
                )]) {
                    sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
                    sh "docker image tag vishnuapp:latest ${dockerHubUser}/vishnu-app:latest"
                    sh "docker push ${dockerHubUser}/vishnu-app:latest"
                }
            }
        }

        stage("deploy") {
            steps {
                sh 'docker rm -f vishnuapp || true'    // optional cleanup
                sh 'docker run -d --name vishnuapp -p 8081:8080 vishnuapp'
            }
        }
    }
}

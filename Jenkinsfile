pipeline {
    agent {
        node {
            label "Dev"
        }
    }
    stages {
       stage("Code") {
           steps {
            git url: "https://github.com/nehabhardwaj1507/two-tier-flask-app.git", branch: "master"
           echo "Code is clonned"
           }
       }
       stage("Build & Test") {
           steps {
                      
            sh "docker build -t flaskimg:latest ." 
           echo "Code is built and tested"
           }
       }
       stage("Credentials") {
           steps {
                echo "Credentials for username and Password added"
                withCredentials(
                    [usernamePassword(
                        credentialsId: "dockerhubcreds", 
                        passwordVariable: "dockerHubPass", 
                        usernameVariable: "dockerHubUser"
                        )
                    ]
                )
                {
                sh "docker image tag flaskimg:latest ${env.dockerHubUser}/flaskimg-jenkins:latest"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/flaskimg-jenkins:latest"
                }
           }
       }
       stage("Deploy") {
           steps {
                sh "docker-compose down && docker-compose up -d"
                echo "Application is deployed"
           }
       }
    }
}

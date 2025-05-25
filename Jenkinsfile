pipeline{
    
    agent { label "dev"};

stages{
    stage("Code"){
        steps{
            git url: "<https://github.com/ishaanzatey/two-tier-flask-app.git>", branch: "master"
        }
    }
    stage("build"){
        steps{
            sh "docker build -t two-tier-flask-app ."
        }
    }
    stage("Push to Docker Hub"){
        steps{
            withCredentials([usernamePassword(
                credentialsId: "dockerHubCreds",
                passwordVariable: "dockerHubPass",
                usernameVariable: "dockerHubUser"
            )]){
            sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
            sh "docker image tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app"
            sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
            }
        }
    }
    stage("deploy"){
        steps{
            sh "docker compose up -d --build flask-app"
        }
    }
}

}

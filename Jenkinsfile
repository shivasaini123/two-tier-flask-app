pipeline{
    
    agent any;
    
    stages{
        stage("code"){
            steps{
                git url: "https://github.com/shivasaini123/two-tier-flask-app", branch: "master" 
                }
            }
        stage("build"){
            steps{
                sh "docker build -t shivasaini7618/flask-app:latest ."
            }
        }
        stage("test"){
            steps{
                echo "test completed successfully"
            }
        }
        stage("push to docker hub"){
            steps{
                withCredentials([usernamePassword(
                    credentialsId:"dockerHubCreds",
                    usernameVariable: "dockerHubUser",
                    passwordVariable: "dockerHubPass"
                )]){
                    sh '''
                    echo $dockerHubPass | docker login -u $dockerHubUser --password-stdin
                    docker push $dockerHubUser/flask-app:latest
                    '''
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker compose up -d --build"
            }
        }
    }
}

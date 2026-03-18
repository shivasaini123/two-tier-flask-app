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
                sh "docker build -t 381790627235.dkr.ecr.eu-west-1.amazonaws.com/flask-app:latest ."
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
                    usernameVariable: "AWS_ACCESS_KEY_ID",
                    passwordVariable: "AWS_ACCESS_KEY_ID"
                )]){
                    sh '''
                    AWS_REGION=eu-west-1
                    ECR_REPO=381790627235.dkr.ecr.eu-west-1.amazonaws.com/flask-app

                    # Login to ECR
                    aws ecr get-login-password --region $AWS_REGION | \
                    docker login --username AWS --password-stdin 381790627235.dkr.ecr.eu-west-1.amazonaws.com

                    docker tag flask-app:latest $ECR_REPO:latest
                    docker push $ECR_REPO:latest
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

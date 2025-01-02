pipeline{
    agent any

    tools {
        nodejs 'npm'
    }

    stages{
        stage("CheckoutCode"){
            steps{
                git credentialsId: 'fff19686-8680-4a34-ad7d-9a803002a583', url: 'https://github.com/tss-devops/node-website.git'
            }
        }
        stage("Build"){
            steps{
                sh "npm install"
                sh "npm install -g sonar-scanner"
            }
        }
        stage("SonarQubeReport"){
            steps{
                sh "sonar-scanner -Dsonar.host.url=http://13.127.133.145:9000/ -Dsonar.login=squ_e92a682d348d1eccb4f68effc67d2712e072abbf -Dsonar.projectKey=node-website"
            }
        }
        stage("UploadArtifactToNexus"){
            steps{
                sh "npm publish"
            }
        }
        stage("DeployToNginx"){
            steps{
                sshagent(['e9c2ff42-814e-4a20-9ed9-fa3725c220fb']){
                 sh "scp -o StrictHostKeyChecking=no -r * ubuntu@172.31.9.70:/var/www/node-web/"
                }
            }
        }
    }
}
pipeline {
    agent any
    
    stages {
        stage("code"){
            steps{
                git url: "https://github.com/LondheShubham153/node-todo-cicd.git", branch: "master"
                echo "code is taken"
            }
            
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo "code is build"
            }
        }
        stage("image scan"){
            steps{
                echo "scanned"
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app-test-new:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo "code is pushed" 
                }
            }
            
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo "code is deploy"
            }
            
        }
         stage('Post-Build Actions') {
            post {
                success {
                    // Send email notification on successful builds
                    emailext (
                        recipientList: 'recipient1@example.com, recipient2@example.com',
                        subject: "${JOB_NAME} build - Successful",
                        body: """
                        Build '${JOB_NAME} - ${BUILD_NUMBER}' completed successfully.

                        * Check console output: [Job Console]($BUILD_URL)
                        """
                    )
                }
                failure {
                    // Send email notification on failed builds
                    emailext (
                        recipientList: 'recipient1@example.com, recipient2@example.com',
                        subject: "${JOB_NAME} build - Failed",
                        body: """
                        Build '${JOB_NAME} - ${BUILD_NUMBER}' failed.

                        * Check console output: [Job Console]($BUILD_URL)
                        * Check build logs for more details.
                        """
                    )
                }
    }
}

pipeline {
    agent any
    post {
  success {
    // Send email only on successful builds
    emailext body: 'Build successful for job ${JOB_NAME}',
             subject: 'Job ${J0B_NAME} - Build Success',
             to: 'personalccount14@gmail.com'
  }
  failure {
    // Send email on build failures
    emailext body: 'Build failed for job ${JOB_NAME}',
             subject: 'Job ${JOB_NAME} - Build Failure',
             to: 'personalccount14@gmail.com'
  }
}
    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/anushkaaaa-tomar/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build -t node-app-test-new ."
            }
        }
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app-test-new ${env.dockerHubUser}/node-app-test-new:latest"
                    sh "docker push ${env.dockerHubUser}/node-app-test-new:latest" 
                }
            }
        }
        stage("Deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}

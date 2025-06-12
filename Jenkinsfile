pipeline{
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
    }
    environment {
        AWS_ACCOUNT_ID="913524947518"
        AWS_DEFAULT_REGION="us-west-1" 
        IMAGE_REPO_NAME="parasuram/devzen"
        IMAGE_TAG="${RELEASE}-${BUILD_NUMBER}"
        RELEASE = "latest"
        REPOSITORY_URI = "${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/${IMAGE_REPO_NAME}"
    }
    stages{
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
            // post{
            //     always{
            //         echo "========always========"
            //     }
            //     success{
            //         echo "========A executed successfully========"
            //     }
            //     failure{
            //         echo "========A execution failed========"
            //     }
            // }
        }
        stage("Checkout From SCM"){
            steps{
                git branch: 'master', credentialsId: 'github', url: 'https://github.com/parasu-aws-cloud/register-app'
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }
        stage("Test application"){
            steps{
                sh "mvn test"
            }
        }
        stage("Sonarqube Analysis"){
            steps{
                script{
                     withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                        sh "mvn sonar:sonar"
                     }
                }
                
            }
        }
        stage('Logging into AWS ECR') {
           steps {
             script {
                sh "aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com"
            }
           }
        }
        // Building Docker images
        stage('Building image') {
          steps{
           script {
             dockerImage = docker.build "${IMAGE_REPO_NAME}:${IMAGE_TAG}"
            }
          }
        }
        // stage("Quality Gate"){
        //    steps {
        //        script {
        //             waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonarqube-token'
        //         }	
        //     }

        // }
        // stage('Quality Gate') {
        //     steps {
        //         script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
        //         }
        //     }
        // }
    // post{
    //     always{
    //         echo "========always========"
    //     }
    //     success{
    //         echo "========pipeline executed successfully ========"
    //     }
    //     failure{
    //         echo "========pipeline execution failed========"
    //     }
    // }
    }

}
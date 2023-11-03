pipeline{
    agent{
        label "Jenkins-Agent" // here define a agent
    }
    tools{ // define tools
        jdk 'Java17'
        maven 'Maven3'
    }
    // environment {
    //     APP_NAME = "register-app-pipeline"
    //     RELEASE = "1.0.0"
    //     DOCKER_USER = "parasuramkoppada"
    //     DOCKER_PASS = "jenkins-docker-token"
    //     IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
    //     IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    // }
    
    stages{
        
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
           
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

        // stage("Test application"){
        //     steps{
        //         sh "mvn test"
        //     }
        // }

        //  stage("Sonarqube Analysis"){
        //     steps{
        //         script{
        //              withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
        //                 sh "mvn sonar:sonar"
        //              }
        //         }
                
        //     }
        // }
        
        // stage("Quality Gate"){
        //    steps {
        //        script {
        //             waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
        //         }	
        //     }

        // }

        // stage("Build & Push Docker Image") {
        //     steps {
        //         script {
        //             docker.withRegistry('',DOCKER_PASS) {
        //                 docker_image = docker.build "${IMAGE_NAME}"
        //             }

        //             docker.withRegistry('',DOCKER_PASS) {
        //                 docker_image.push("${IMAGE_TAG}")
        //                 docker_image.push('latest')
        //             }
        //         }
        //     }

    }
}
    

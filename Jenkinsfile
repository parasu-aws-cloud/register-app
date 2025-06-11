pipeline{
    agent { label 'Jenkins-Agent' }
    tools {
        jdk 'Java17'
        maven 'Maven3'
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
        // stage("Quality Gate"){
        //    steps {
        //        script {
        //             waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonarqube-token'
        //         }	
        //     }

        // }
        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonarqube-token'
                }
            }
        }
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
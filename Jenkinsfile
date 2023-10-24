pipeline{
    agent{
        label "Jenkins-agent" // here define a agent
    }
    tools{ // define tools
        jdk 'Java17'
        maven 'Maven3'
    }
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

        stage("Test application"){
            steps{
                sh "mvn test"
            }
        }
    }
    
}
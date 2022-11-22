pipeline{

     agent any

     stages{

        stage('Git Checkout'){

            steps{
                git branch: 'main', url: 'https://github.com/virupabandi/demo-counter-app-e2e-devopsPart2final.git'
            }
        }
        stage('UNIT Testing'){

            steps{
                sh 'mvn test'
            }
        }
        stage('integration test'){

            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage('Maven Build'){

            steps{
                sh 'mvn clean install'
            }
        }
        stage('Static code analysis'){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-api-key') {
                    sh 'mvn clean package sonar:sonar'
                }

              }
            }
        }
        stage('Quality Gate status'){

            steps{

                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-api-key'
                }
            }
        }
        stage('upload war file to nexus'){

            steps{

               
                script{

                     def readPomVersion = readMavenPom file: 'pom.xml'

                     def nexusRepo = readPomVersion.version.endsWith("SNAPSHOT") ? "demoapp-snapshot" : "demoapp-release"
                    nexusArtifactUploader artifacts: 
                [
                    [
                        artifactId: 'springboot', 
                        classifier: '', 
                        file: 'target/Uber.jar', 
                        type: 'jar'
                        ]
                ], 
                credentialsId: 'nexus-auth', 
                groupId: 'com.example', 
                nexusUrl: '35.175.216.52:8081', 
                nexusVersion: 'nexus3', 
                protocol: 'http', 
                repository: nexusRepo, 
                version: "${readPomVersion.version}"
                }
            }
        }
     }     
}

    
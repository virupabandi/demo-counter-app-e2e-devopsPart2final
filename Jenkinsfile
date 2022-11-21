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
     }     
}

    
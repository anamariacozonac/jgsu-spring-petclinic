pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/anamariacozonac/jgsu-spring-petclinic'
            }
        }
        stage('Build') {
            steps {
                // Run Maven on a Unix agent.
                sh './mvnw clean package'
                //sh 'true'

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }


            post {
                always {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                //}
               // changed {
                    emailext attachLog: true,
                    body: 'Please go to ${BUILD_URL} and verify the build',
                    compressLog: true,
                    recipientProviders: [upstreamDevelopers(), requestor()],
                    subject: 'Job \'${JOB_NAME}\' (${BUILD_NUMBER}) is waiting for input',
                    to: 'maria30.cozonac@gmail.com'
                }
            }
        }
    }
}

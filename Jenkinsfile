pipeline {
    agent any
    
     triggers { pollSCM('H/15 * * * *') } // poll every 15 minutes

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }
     environment {
       // use your actual issuer URL here and NOT the placeholder {yourOktaDomain}
       OKTA_OAUTH2_ISSUER           = 'https://dev-16227572.okta.com/oauth2/default'
       OKTA_OAUTH2_CLIENT_ID        = credentials('OKTA_OAUTH2_CLIENT_ID')
       OKTA_OAUTH2_CLIENT_SECRET    = credentials('OKTA_OAUTH2_CLIENT_SECRET')
   }

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/Tyagi-S/okta-spring-security-example-app.git'

                // Run Maven on a Unix agent.
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}

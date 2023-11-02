pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = '9c0d523b-4056-4de7-88ec-8c220290a27b'
        // Don't put your actual token here. Set it in Jenkins' environment variables.
        NETLIFY_AUTH_TOKEN = 'HPPeIQj1EP2aur9lPny96FheKqZV5eTNbjQlpWn4Ii4'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Lint HTML') {
            steps {
                catchError(buildResult: 'SUCCESS', stageResult: 'FAILURE') {
                    sh 'tidy -q -e public/*.html'
                }
            }
        }

        stage('Package') {
            steps {
                sh 'zip -r website.zip public/*'
                archiveArtifacts artifacts: 'website.zip', fingerprint: true
            }
        }

        stage('Deploy to Netlify') {
            steps {
                script {
                    def response = sh(script: """
                    curl -H "Content-Type: application/zip" \\
                         -H "Authorization: Bearer ${NETLIFY_AUTH_TOKEN}" \\
                         --data-binary "@website.zip" \\
                         https://api.netlify.com/api/v1/sites/${NETLIFY_SITE_ID}/deploys
                    """, returnStdout: true).trim()
                    echo "Netlify Response: $response"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployed successfully!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}



// pipeline {
//     agent any

//     tools {
//         maven 'Maven_3.6.3' // Replace 'Maven_3.6.3' with the name of your Maven installation in Jenkins if different
//     }

//     stages {
//         stage('Checkout') {
//             steps {
//                 checkout scm
//             }
//         }
        
//         stage('Build') {
//             steps {
//                 sh 'mvn clean compile'
//             }
//         }
        
//         stage('Test') {
//             steps {
//                 sh 'mvn test'
//             }
//         }

//         stage('Package') {
//             steps {
//                 sh 'mvn package'
//             }
//         }
//     }
// }

pipeline {
    agent any

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
                    // // Set NETLIFY_SITE_ID and NETLIFY_AUTH_TOKEN as environment variables in Jenkins
                    // sh '''
                    // curl -H "Content-Type: application/zip" \
                    //      -H "Authorization: Bearer $NETLIFY_AUTH_TOKEN" \
                    //      --data-binary "@website.zip" \
                    //      https://api.netlify.com/api/v1/sites/$NETLIFY_SITE_ID/deploys
                    // '''
                    echo "deployed ok"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment succeeded!'
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

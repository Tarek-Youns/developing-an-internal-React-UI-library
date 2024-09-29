pipeline { 
    agent any

    environment {
        NODE_VERSION = '18.6.0'
        NEXUS_TOKEN = credentials('NEXUS_TOKEN') 
        NEXUS_REPO = "http://localhost:8081/repository/npm-nexus-repo/" 

    }

    options {
        skipStagesAfterUnstable()
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Tarek-Youns/developing-an-internal-React-UI-library.git' 
            }
        }

        stage("npm packages handling") {
            steps {
                cache(caches: [
                    arbitraryFileCache(
                        path: "node_modules",
                        includes: "**/*",
                        cacheValidityDecidingFile: "package-lock.json"
                    )
                ]
                )
                 {
                    sh "npm ci"  
                }
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'npm test'
            }
        }

        stage('Publish') {
            when {
                branch 'main'
            }
            steps {
                script {
                    withEnv(["NEXUS_TOKEN=${NEXUS_TOKEN}", "NEXUS_REPO=${NEXUS_REPO}"]) {
                        
                        sh '''
                        npm set //$NEXUS_REPO/:_authToken=$NEXUS_TOKEN 
                        npm publish --registry=$NEXUS_REPO

                        '''
                    }
                }
            }
        }
    }
    

    post {
        always {
            // Clean up workspace after build
            cleanWs()
        }
    }
}

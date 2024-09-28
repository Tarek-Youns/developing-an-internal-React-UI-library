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

        stage('Restore Cache') {
            steps {
                script {
                    try {
                        // Try to restore the cached node_modules
                        unstash 'node_modules_cache'
                    } catch (Exception e) {
                        echo "Cache miss, installing dependencies..."
                    }
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies if not cached
                script {
                    if (!fileExists('node_modules')) {
                        sh 'npm ci'
                    }
                }
            }
        }

        stage('Save Cache') {
            steps {
                // Save the node_modules to cache
                script {
                    stash includes: 'node_modules/**', name: 'node_modules_cache'
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

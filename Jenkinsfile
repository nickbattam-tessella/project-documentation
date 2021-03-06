#!groovy

pipeline {
    agent {
        label 'PACE Windows (Private)'
    }
    stages {
        stage('Checkout') { 
            steps {
                bat '''
                    git clone https://github.com/pace-neutrons/document-test . &
                    git checkout %BRANCH_NAME%
                '''
            }
        }
        stage('Prepare') {
            steps {
                bat '''
                    git rm -r docs
                '''
            }
        }
        stage('Build') { 
            steps {
                bat '''
                    pip install sphinx &
                    pip install sphinx_rtd_theme &
                    make.bat html &
                    make.bat html
                '''
            }
        }
        stage('Deploy') { 
            steps {    
                withCredentials([string(credentialsId: 'GitHub_API_Token', 
                            variable: 'api_token')]) {
                    bat '''
                        git add docs
                        git commit -m "Document build from CI"
                        git config --local user.name "PACE CI Build Agent"
                        git remote set-url origin https://pace-builder:%api_token%@github.com/pace-neutrons/document-test.git
                        git push origin %BRANCH_NAME%
                    '''
                }
            }
        }
    }
}
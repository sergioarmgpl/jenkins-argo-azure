pipeline {
    agent any
    environment { 
        APP_VERSION = '0.0.1'
        IMG_NAME = 'app1'
        ARGOCD_SERVER='argocd.cnca.io'
        ARGO_PROJECT='app1'
        NAMESPACE='dev'
        AZ_REGISTRY='codecamp2021.azurecr.io'
        DOMAIN='app1.demo.cnca.io'
    }
    stages {
        stage('Build & Test Dev') {
            steps {
                script {
                    withCredentials([    
                        usernamePassword(credentialsId: 'azure_credentials', usernameVariable: 'AZ_USER', passwordVariable: 'AZ_PASSWORD')
                    ]) {
                        dir('src'){
                            sh 'echo $GIT_BRANCH'
                            sh "make push_az"
                            sh "make clean"
                        }                        
                    }
                }                
            }
        }
        stage('Deploy with Argo') {
            steps {
                script {
                    string(credentialsId: 'argo_token', variable: 'ARGO_TOKEN')
                    ]) {

                        dir('src'){
                            env.GIT_COMMIT_MSG = sh (script: 'git log -1 --pretty=%B ${GIT_COMMIT}', returnStdout: true).trim()
                            sh "make deploy_argo"
                        }
                    }    
                }                
            }
        } 
    }
}

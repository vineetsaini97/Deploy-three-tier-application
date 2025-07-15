pipeline {
    agent any

    environment {
        GITHUB_USER = 'vineetsaini97'
        GITHUB_REPO = 'deploy-three-tier-application' // must match repo name exactly, all lowercase
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', credentialsId: 'GITHUB_CREDS', url: "https://github.com/${env.GITHUB_USER}/${env.GITHUB_REPO}.git"
            }
        }

        stage('Build and Push to GHCR') {
            steps {
                withCredentials([string(credentialsId: 'GHCR_TOKEN', variable: 'GITHUB_PAT')]) {
                    sh '''
                        echo $GITHUB_PAT | docker login ghcr.io -u $GITHUB_USER --password-stdin

                        docker build -t ghcr.io/$GITHUB_USER/$GITHUB_REPO/frontend:latest ./frontend
                        docker build -t ghcr.io/$GITHUB_USER/$GITHUB_REPO/backend:latest ./backend

                        docker push ghcr.io/$GITHUB_USER/$GITHUB_REPO/frontend:latest
                        docker push ghcr.io/$GITHUB_USER/$GITHUB_REPO/backend:latest
                    '''
                }
            }
        }
    }
}


// pipeline {
//     agent any

//     environment {
//         GITHUB_USER = 'vineetsaini97'
//         GITHUB_REPO = 'deploy-three-tier-application'
//     }

//     stages {
//         stage('Checkout Code') {
//             steps {
//                 git branch: 'main', credentialsId: 'GITHUB_CREDS', url: 'https://github.com/vineetsaini97/deploy-three-tier-application.git'
//             }
//         }

//         stage('Build and Push to GHCR') {
//             steps {
//                 withCredentials([string(credentialsId: 'GHCR_TOKEN', variable: 'GITHUB_PAT')]) {
//                     sh '''
//                         echo $GITHUB_PAT | docker login ghcr.io -u $GITHUB_USER --password-stdin

//                         docker build -t frontend ./frontend
//                         docker build -t backend ./backend

//                         docker tag frontend ghcr.io/$GITHUB_USER/$GITHUB_REPO/frontend:latest
//                         docker tag backend ghcr.io/$GITHUB_USER/$GITHUB_REPO/backend:latest

//                         docker push ghcr.io/$GITHUB_USER/$GITHUB_REPO/frontend:latest
//                         docker push ghcr.io/$GITHUB_USER/$GITHUB_REPO/backend:latest
//                     '''
//                 }
//             }
//         }
//     }
// }

//         stage('Deploy MySQL') {
//             steps {
//                 bat 'kubectl apply -f k8s/mysql-deployment.yaml'
//                 bat 'kubectl apply -f k8s/mysql-service.yaml'
//             }
//         }

//         stage('Deploy phpMyAdmin') {
//             steps {
//                 bat 'kubectl apply -f k8s/phpmyadmin-deployment.yaml'
//                 bat 'kubectl apply -f k8s/phpmyadmin-service.yaml'
//             }
//         }

//         stage('Deploy Backend') {
//             steps {
//                 bat 'kubectl apply -f k8s/backend-deployment.yaml'
//                 bat 'kubectl apply -f k8s/backend-service.yaml'
//             }
//         }

//         stage('Deploy Frontend') {
//             steps {
//                 bat 'kubectl apply -f k8s/frontend-deployment.yaml'
//                 bat 'kubectl apply -f k8s/frontend-service.yaml'
//             }
//         }
//     }
// }

pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                script {
                    cleanWs()
                }
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/mohyehia/spring-microservices-in-action-manifests'
            }
        }

        stage('Update K8s Manifests') {
            steps {
                sh "cat ${APPLICATION}/manifest.yml"
                sh "sed -i 's|${DOCKER_IMAGE}.*|${DOCKER_IMAGE}:${BUILD_VERSION}|g' ${APPLICATION}/manifest.yml"
                sh "cat ${APPLICATION}/manifest.yml"
            }
        }

        stage('Push Updated Manifests') {
            steps{
                sh """
                    git config user.email "mohammedyehia99@gmail.com"
                    git config user.name "mohyehia"
                    git add ${APPLICATION}/manifest.yml
                    git commit -m "Update manifest.yml file"
                """
                withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'gitPassword', usernameVariable: 'gitUsername')]) {
                    sh "git push https://${env.gitUsername}:${env.gitPassword}@github.com/mohyehia/spring-microservices-in-action-manifests.git main"
                }
            }
        }
    }
}
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
                sh "cat spring-boot-testing/manifest.yml"
                sh "sed -i 's|${DOCKER_IMAGE}.*|${DOCKER_IMAGE}:${BUILD_VERSION}|g' spring-boot-testing/manifest.yml"
                sh "cat spring-boot-testing/manifest.yml"
            }
        }

        stage('Push Updated Manifests') {
            steps{
                sh """
                    git config user.email "mohammedyehia99@gmail.com"
                    git config user.name "mohyehia"
                    git add spring-boot-testing/manifest.yml
                    git commit -m "Update manifest.yml file"
                """
                withCredentials([usernamePassword(credentialsId: 'github-token', passwordVariable: 'gitPassword', usernameVariable: 'gitUsername')]) {
                    sh "git push https://${env.gitUsername}:${env.gitPassword}@github.com/mohyehia/spring-microservices-in-action-manifests.git main"
                }
            }
        }
    }
}
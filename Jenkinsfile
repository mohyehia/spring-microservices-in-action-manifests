pipeline {
    agent any

    environment {
        BUILD_VERSION = '0.0.7'
        DOCKER_IMAGE = 'mohyehia99/spring-boot-testing'
    }

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
    }
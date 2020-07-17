pipeline {
    agent any
    /*
    // master-slave 구성 후 slave node에서 해당 Item을 구동할 경우
    agent {
        node {
            label 'slave'
        }
    }
    */
    /*
    environment {
            IMG_PREFIX = readMavenPom().getProperties().getProperty('docker.image.prefix')
            IMAGE_NAME = readMavenPom().getArtifactId()
            VERSION = readMavenPom().getVersion()
    }
    */

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        /*
        stage('Build Docker Image') {

            steps {
                sh 'mvn install dockerfile:build'
                sh 'docker tag ${IMG_PREFIX}/${IMAGE_NAME} gcr.io/${IMG_PREFIX}/${IMAGE_NAME}'
            }
        }
        */

        stage('Quality Analysis') {
            steps {
                withSonarQubeEnv('widerplanet SonarQube'){
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Response Quality Gate') {
          steps {
            timeout(time: 5, unit: 'MINUTES'){
                waitForQualityGate(abortPipeline: true, credentialsId: 'e475a9c8-3679-463f-8139-5bf21e37eb10')
            }
          }
        }

        /*
        stage('Push Image to GCR') {
             steps {
                 withDockerRegistry([credentialsId: 'gcr:Wider Planet DevOps', url: 'https://gcr.io']){
                    sh "docker push gcr.io/${IMG_PREFIX}/${IMAGE_NAME}:latest"
                 //sh "docker push ${env.DOCKER_REPO}/${env.DOCKER_IMAGE}:${env.BUILD_NUMBER}"
                 }
             }
        }
        */
    }
}

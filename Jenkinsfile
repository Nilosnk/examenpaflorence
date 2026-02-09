pipeline {
    agent any

    stages {

        stage('Clonar repositorio') {
            steps {
                checkout scm
            }
        }

        stage('Construir imagen Docker') {
            steps {
                sh 'docker build -t isaacarenas23/examenpaflorence:latest .'
            }
        }

        stage('Subir imagen a Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'docker_hub', variable: 'DOCKER_PASSWORD')]) {
                    sh '''
                        echo $DOCKER_PASSWORD | docker login -u isaacarenas23 --password-stdin
                        docker push isaacarenas23/examenpaflorence:latest
                    '''
                }
            }
        }

        stage('Desplegar en Kubernetes') {
            steps {
                kubernetesDeploy(
                    configs: 'deployment.yaml',
                    kubeconfigId: 'kubernetes_config'
                )
            }
        }
    }
}

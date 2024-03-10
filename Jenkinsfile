agent any

stages {
    stage('Checkout') {
        steps {
            git branch: 'master', credentialsId: 'git-threepoints-github', url: 'https://github.com/mpcevallos/practica_pipeline'
        }
    }

    stage('Pruebas de SAST') {
        steps {
            script {
                withSonarQubeEnv(credentialsId:'token') {
                    sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=sonarqube \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.login=sqp_8871e861546564ca35025574380ccc281e056c0c
                    """
                }
                timeout(time: 10, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
    }

    stage('Build') {
        steps {
            sh 'docker build -t devops_threepoints:latest .'
        }
    }
}

post {
    success {
        echo 'Pipeline exitoso'
    }
    failure {
        echo 'Pipeline fallido'
    }
}

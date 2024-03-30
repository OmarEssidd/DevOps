pipeline {
    agent any

    stages {
        stage('GIT') {
            steps {
                echo "Getting project from Git"
                
                // Récupérer le code depuis GitHub
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/master']], 
                          doGenerateSubmoduleConfigurations: false, 
                          extensions: [[$class: 'CleanCheckout']], 
                          submoduleCfg: [], 
                          userRemoteConfigs: [[url: 'https://github.com/OmarEssidd/devops.git']]])
            }
        }

        stage('MVN CLEAN') {
            steps {
                // Nettoyer le code avec Maven
                sh 'mvn clean'
            }
        }

        stage('MVN COMPILE') {
            steps {
                // Compiler le code avec Maven
                sh 'mvn compile'
            }
        }

        stage('MVN SONARQUBE') {
            steps {
                // Analyser la qualité du code avec SonarQube
                withSonarQubeEnv('MonInstanceSonarQube') {
                    sh '''
                    mvn sonar:sonar \
                        -Dsonar.login=admin \
                        -Dsonar.password=${credentials('sonarqube_token')}
                    '''
                }
            }
        }
    }
}

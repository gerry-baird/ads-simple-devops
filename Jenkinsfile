pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                        echo "Working Directory "
                        echo $PWD
                '''
            }
         }
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'mvn -s maven-settings.xml clean install'
            }
        }
        stage('Test') {
            steps {
                echo 'Skipping Tests..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
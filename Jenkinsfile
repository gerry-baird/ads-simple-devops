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
                withCredentials([usernamePassword(credentialsId: '051df11b-157b-44d5-a7d5-971f71b5efa5', passwordVariable: 'nexusPwd', usernameVariable: 'nexusUser')]) {
                    sh 'mvn -X -s maven-settings.xml clean install -f Simple/pom.xml'
                }

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
                withCredentials([string(credentialsId: 'ZenApiKey-Dev', variable: 'ZenApiKey')]) {
                    sh '''
                        echo "zen key = ${ZenApiKey}"
                        echo "Running ADS Deployment using REST from Jenkinsfile"
                        echo "Build Number ${BUILD_NUMBER}"
                        curl -k -X 'POST' \
                        "https://cpd-cp4ba.mycluster-eu-gb-1-cx2-16x-4d2c0e6e364e1cb6bda1360a996d18f0-0000.eu-gb.containers.appdomain.cloud/ads/runtime/api/v1/deploymentSpaces/embedded/decisions/ads-simple-pipeline-uat2-${BUILD_NUMBER}/archive" \
                        -H 'accept: */*' \
                        -H "Authorization: ZenApiKey ${ZenApiKey}" \
                        -H 'Content-Type: application/octet-stream' \
                        --data-binary @/var/lib/jenkins/.m2/repository/cpadmin/gb_simple_devops/simple/simpleDecisionService/1.0.0-SNAPSHOT/simpleDecisionService-1.0.0-SNAPSHOT.jar
                    '''
                }

            }
        }
    }
}

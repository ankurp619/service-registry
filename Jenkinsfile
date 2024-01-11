pipeline {
   agent any
   tools {
        maven 'maven-3.9.6'
   }
   stages {
        stage('Checkout') {
              steps {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins-pat', url: 'https://github.com/ankurp619/service-registry.git']])
              }
        }
        stage('Build') {
            steps {
                sh "mvn clean install"
            }
        }
        stage('Docker') {
             steps {
                  script {
                        withCredentials([usernamePassword(credentialsId: jenkinsCredId, passwordVariable: 'REGISTRY_PASSWORD', usernameVariable: 'REGISTRY_USERNAME')]) {
                            sh """
                                  docker login -u ${REGISTRY_USERNAME} -p ${REGISTRY_PASSWORD} ankurp619/ankur
                                  docker build -t ankurp619/ankur/Springboot-Microservice/service-registry:${gitBranchOrTagName} .
                                  docker push ankurp619/ankur/Springboot-Microservice/service-registry:${gitBranchOrTagName}
                              """
                        }
                  }
             }
        }
   }
}
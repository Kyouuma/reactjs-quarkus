pipeline {
  agent {
    kubernetes {
      label 'quarkus-reactjs-builder'
      idleMinutes 5
      yamlFile 'build-pod.yaml'
      defaultContainer 'dind'
    }
  }
  options {
        disableConcurrentBuilds()
        buildDiscarder(logRotator(daysToKeepStr: '10', numToKeepStr: '5', artifactNumToKeepStr: '1'))
   }
  stages {
        stage('Build') {
                    steps {
                        sh "docker build --tag kyouuma/reactjs-quarkus:base -f dockerfiles/Dockerfile-data ."
                    }
                }
        stage('Deploy') {
                    environment{
                          DOCKER = credentials("docker-hub")
                      }
                    steps {
                        sh "docker login -u={$DOCKER_USR} -p=${DOCKER_PSW}"
                        sh "docker push  kyouuma/reactjs-quarkus:base "
                    }
                } 
        } 
  }

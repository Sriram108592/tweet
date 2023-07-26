def registry = 'https://bhaskaram.jfrog.io'
def imageName = 'bhaskaram.jfrog.io/docker-docker/ttrend_new'
def version   = '2.1.2'
pipeline {
    agent {
        label 'slave'
    }
    environment {
        PATH = "/opt/apache-maven-3.9.3/bin:$PATH"
    }
    stages {
        stage(" Deploy ") {
       steps {
         script {
            echo '<--------------- Helm Deploy Started --------------->'
            sh 'helm install ttrend-v1 Sriram108592/tweet/blob/main/ttrend-0.1.0.tgz'
            echo '<--------------- Helm deploy Ends --------------->'
         }
       }
     }
    }
}

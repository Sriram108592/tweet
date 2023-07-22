def imageName = 'bhaskaram.jfrog.io/artifactory/docker-docker/ttrend'
def version   = '2.1.2'
pipeline {
    agent {
        node {
            label 'slave'
        }
    }
    environment {
        PATH = "/opt/apache-maven-3.9.2/bin:$PATH"
    }
    stages {
        stage('deploy') {
            steps {
                sh 'mvn clean deploy'
            }
        }

        stage(' Docker Build ') {
            steps {
                script {
                    echo '<--------------- Docker Build Started --------------->'
                    app = docker.build(imageName + ':' + version)
                    echo '<--------------- Docker Build Ends --------------->'
                }
            }
        }
        stage(' Docker Publish ') {
            steps {
                script {
                    echo '<--------------- Docker Publish Started --------------->'
                    docker.withRegistry(registry, 'jfrog-cred') {
                        app.push()
                    }
                    echo '<--------------- Docker Publish Ended --------------->'
                }
            }
        }
    }
}

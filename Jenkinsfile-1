def registry = 'https://bhaskaram.jfrog.io'
def imageName = 'bhaskaram.jfrog.io/docker-docker/ttrend'
def version   = '2.1.2'
pipeline {
    agent {
        label 'slave'
    }
    environment {
        PATH = "/opt/apache-maven-3.9.3/bin:$PATH"
    }
    stages {
        stage('deploy') {
            steps {
                echo '--------build started--------'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '--------build completed--------'
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

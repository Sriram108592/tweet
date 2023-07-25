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
        tage('test') {
            steps {
                echo '-----unit test started--------'
                sh 'mvn surefire-report:report'
                echo '-----unit test completed--------'
            }
        }
    }
}

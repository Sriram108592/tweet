def registry = 'https://bhaskaram.jfrog.io'
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
    }
}

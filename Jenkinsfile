pipeline{
    agent{
        label 'slave'
    }
    environment{
        PATH = '/opt/apache-maven-3.9.3/bin:$PATH'
    }
    stages {
        stage('build') {
            steps {
                sh "mvn clean install"
            }
        }
    }
}
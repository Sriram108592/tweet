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
        stage('test') {
            steps {
                echo '-----unit test started--------'
                sh 'mvn surefire-report:report'
                echo '-----unit test completed--------'
            }
        }
                stage('SonarQube analysis') {
            environment {
                scannerHome = tool 'bhaskaram-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('sonar-server') { // If you have configured more than one global server connection, you can specify its name
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Quality Gate') {
            steps {
                echo '-----Quality gate started--------'
                script {
                    timeout(time: 1, unit: 'HOURS') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
                echo '-----Quality gate completed--------'
            }
        }
    }
}

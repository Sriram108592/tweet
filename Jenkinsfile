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
        stage('Build') {
            steps {
                echo '--------build started--------'
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo '--------build completed--------'
            }
        }
        stage('Test') {
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
        //
        stage("Jfrog") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "bns-libs-release/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }
        //
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
        //
  stage(" Deploy ") {
       steps {
         script {
            echo '<--------------- Helm Deploy Started --------------->'
            sh 'helm install ttrend-v1 /home/ubuntu/ttrend-0.1.0.tgz'
            echo '<--------------- Helm deploy Ends --------------->'
         }
       }
     } 
        //
    }
}

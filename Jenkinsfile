pipeline {
    agent {label "my-agent"}
    stages {
        stage ("git clone") {
            steps {
                git url: 'https://github.com/tejesh555/applogin.git'
            }
        }
        stage ("build") {
            steps { 
                sh "mvn clean install"
            }
        }
        stage ("test") {
            steps {
                script {
                    def scannerHome = tool 'myscanner';
                    withSonarQubeEnv('mysonar') {
                    sh "${scannerHome}/bin/sonar-scanner \
                    -D sonar.login=admin \
                    -D sonar.password=password \
                    -D sonar.projectKey=applogin \
                    -D sonar.exclusions=vendor/**,resources/**,**/*.java \
                    -D sonar.host.url=http://35.174.115.121:9000/"
                    }
                }
            }
        }
        stage ("publish") {
            steps {
                script {
                  /*  rtUpload (
                        serverId: 'my-jfrog',
                        spec: '''
                            {
                                "files": [
                                    {
                                        "pattern": "target/*.war",
                                        "target": "applogin"
                                    }
                                ]
                            }
                        ''',
                        buildName: "${JOB_NAME}",
                        buildNumber: "${BUILD_NUMBER}"
                    ) */
                    println "piublish"
                }
            }
        }
    }
}

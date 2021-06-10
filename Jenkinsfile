pipeline {
    agent {label "slave555"}
    
    stages {
        
        stage ("git clone") {
            steps {
                git credentialsId: 'githubid', url: 'https://github.com/tejesh555/applogin.git'
            }
        }

        stage ("build") {
            steps {
                sh "mvn clean install"
            }
        }

        stage ("test") {
            steps {
                echo "this is related to testing"
            }
        }

        stage ("publish") {
            steps {
                step {
                    try {
                        rtUpload ( 
                            serverId: 'myjfrog',
                            spec: '''
                                  {
                                      "files": [
                                          {
                                              "pattern": "target/*.war",
                                              "target": "myapp/"
                                          }
                                      ]
                                  }
                            ''',
                            buildName: "${JOB_NAME}",
                            buildNumber: "${BUILD_NUMBER}"
                        )
                    }
                    catch(Exception ex) {
                        println "unable to push the artifact kindly check the configuration"
                    }
                }
            }
        }

        stage ("deploy") {
            steps {
                echo "ansible-playbook -i inventory e2e.yml"
            }
        }

    }

    post {
        always {
             /*emailext body: '''this is status of

job: "${JOB_NAME}"
url: "${BUILD_URL}"''', subject: 'Status of "{$JOB_NAME}"', to: 'tejesh2311@gmail.com'
        } */
        echo "post action"
        }
    }
}

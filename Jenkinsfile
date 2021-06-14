properties([pipelineTriggers([githubPush()])])

pipeline {
    agent {label "slave555"}
    
    stages {
        
        stage ("git clone project url") {
            steps {
                git url: 'https://github.com/tejesh555/applogin.git'
                sh "ls -all"
            }
        }
        
        stage ("git clone ansible-url") {
            steps {
                git url: "https://github.com/tejesh555/ansible1.git"
                sh "ls -all"
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
                script {
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
                    catch(err) {
                        println "unable to push the artifact kindly check the configuration"
                        println err.getMessage()
                    }
                }
            }
        }

        stage ("deploy") {
            steps {
                sh "ansible-playbook -i tom_host cd.yml"
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

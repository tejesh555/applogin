def pipeline_var = "test_var"
properties([pipelineTriggers([githubPush()])])

pipeline {
   // agent {label 'applogin_slave'}
    agent any
    parameters {
        choice (name: 'git_branch', choices:["master", "develop", "INT", "UAT"])
    }
    stages {
        stage ("clone") {
            steps {
               
                git  url: 'https://github.com/tejesh555/applogin.git'
                 sh """
                   rm -rf /tmp/ansible1
                  cd /tmp
                  git clone https://github.com/tejesh555/ansible1.git
                  """
            }
        }

        stage ("maven build") {
            steps {
                sh "mvn clean install"
            }
        } 

        stage ("test") {
            steps {
                script {
                    try {
                        println "this is for test"
                        println "${pipeline_var}"
                    }
                    catch(err) {
                        println "there is a issue in test"
                    }
                }
            }
        }
/*
        stage ("publish") {
            steps {
                rtUpload (
                    serverId: 'myjfrog',
                    spec: ''' {
                        "files": [
                            {
                            "pattern": "target/*.war",
                            "target": "applogin"
                            }
                        ]
                    } ''',
                        buildName: "${JOB_NAME}",
                        buildNumber: "${BUILD_NUMBER}"

                )
            }
        }
        */

        stage ("deploy") {
            steps {
               script {
                    if ( "${git_branch}" == "master") {
                        sh "ansible-playbook -i /tmp/ansible1/host.prod /tmp/ansible1/end-end.yml"
                    }
                    else if ( "${git_branch}" == "INT") {
                        sh "ansible-playbook -i host.${git_branch} end-end.yml"
                    }
                    else if ( "${git_branch}" == "UAT") {
                       sh "ansible-playbook -i host.${git_branch} end-end.yml"
                    }                    
                    else if ( "${git_branch}" == "develop") {
                       sh "ansible-playbook -i host.${git_branch} end-end.yml"
                    }
                    else {
                        sh "echo 'deploy fail'"
                    }
                }
            }
        }

    }   
}

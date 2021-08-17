def pipeline_var = "test_var"
properties([pipelineTriggers([githubPush()])])

pipeline {
    agent any
    parameters {
        choice (name: 'git_branch', choices:["master", "develop", "INT", "UAT"])
    }
    stages {
        stage ("clone") {
            steps {
                git credentialsId: 'github', url: 'https://github.com/tejesh555/applogin.git'
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

        stage ("deploy") {
            steps {
                script {
                    if ( "${git_branch}" == "master") {
                        println "this is  master"
                    }
                    else if ( "${git_branch}" == "INT") {
                        println "this is  INT"
                    }
                    else if ( "${git_branch}" == "UAT") {
                        println "this is UAT"
                    }                    
                    else if ( "${git_branch}" == "develop") {
                        println "this is develop"
                    }
                    else {
                        println "this is wrong"
                    }
                }
            }
        }

    }   
}

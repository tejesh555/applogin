    pipeline {
        agent any
        stages {
            stage ("clone") {
                steps {
                    git credentialsId: 'tej-git', url: 'https://github.com/tejesh555/applogin.git'
                }
            }
            stage ("build") {
                steps {
                    sh "mvn clean install"
                }
            }
            stage ("test") {
                steps {
                    echo "test"
                }
            }
            stage ("jfrog") {
                steps {
                    script {
                        rtUpload (
                            serverId: 'myjfrog',
                            spec: '''{
                                "files": [
                                    {
                                    "pattern": "target/*.war",
                                    "target": "applogin"
                                    }
                                ]
                            }''',
                            buildName: 'applogin',
                            buildNumber: "${BUILD_NUMBER}"
                        )
                    }
                }
            }
        }
    }

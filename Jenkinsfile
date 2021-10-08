pipeline {
    agent any
    stages {
        stage ("clone") {
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
                echo "test this is in master"
            }
        }
        /*
        stage ("publish") {
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
        } */
        stage ("deploy") {
            agent {label 'ansible'}
            steps {
                sh "mkdir ansible"
                dir('ansible') {
                    sh "pwd"
                    git url: https://github.com/tejesh555/ansible2.git
                }
                sh "ansible-playbook -i hosts e2e.yml"
            }    
        }
    }
}

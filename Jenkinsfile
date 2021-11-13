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
        stage ("publish") {
            steps {
                script {
                    rtUpload (
                        serverId: 'my-jfrog',
                        spec: '''{
                            "files": [
                                {
                                "pattern": "target/*.war",
                                "target": "myapp"
                                }
                            ]
                        }''',
                        buildName: 'applogin',
                        buildNumber: "${BUILD_NUMBER}"
                    )
                }
            }
        } 
        stage ("deploy") {
            steps {
                println "deploy"
            }
        }
        /*
        stage ("deploy") {
            steps {
                script {
                    sh "mkdir ansible"
                    dir('ansible') {
                        sh "pwd"
                        git url: 'https://github.com/tejesh555/ansible2.git'
                    }
                    sh "ansible-playbook -i ansible/host ansible/e2e.yml"
                }
            }    
        } */
    } 
    post { 
        always { 
            cleanWs()
        }
    }
}

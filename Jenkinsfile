pipeline {
    agent { label "myslave" }
    stages {
        stage ("scm") {
            steps {
                git credentialsId: 'github_id', url: 'https://github.com/tejesh555/applogin.git'
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
                    def scannerHome = tool 'mysonarscanner';
                            withSonarQubeEnv('mysonar') {
                            sh "${scannerHome}/bin/sonar-scanner \
                            -D sonar.login=admin \
                            -D sonar.password=Jul@2022 \
                            -D sonar.projectKey=applogin \
                            -D sonar.exclusions=vendor/**,resources/**,**/*.java \
                            -D sonar.host.url=http://3.20.232.56:9000/"
                        }
                }
            }
        }
        stage ("release") {
            steps {
                script {
                    rtUpload (
                        serverId: 'myjfrog',
                        spec: '''{
                            "files": [  
                                {
                                "pattern": "target/*.war",
                                "target": "myapplogin"
                                }
                            ]
                        }''',
                    
                        // Optional - Associate the uploaded files with the following custom build name and build number,
                        // as build artifacts.
                        // If not set, the files will be associated with the default build name and build number (i.e the
                        // the Jenkins job name and number).
                        buildName: '${BUILD_DISPLAY_NAME }',
                        buildNumber: '${ BUILD_NUMBER }',
                        // Optional - Only if this build is associated with a project in Artifactory, set the project key as follows.
                        project: '${JOB_NAME}'
                    )
                }
            }
        }
    }
}

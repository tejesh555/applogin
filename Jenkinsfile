pipeline {
  agent {label 'myslave'}
  
  stages {
      stage ("clone") {
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
                    println "test"
            }
        }
        stage ( "release") {
            steps {
                script {
                    rtUpload (
                        serverId: 'myjfrog',
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
                    )
                }
            }
        }
   }
}

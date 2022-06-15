pipeline {
	agent { label 'myAgent' }
	stages {
		stage ("clone") {
			steps {
				git credentialsId: 'github', url: 'https://github.com/tejesh555/applogin.git'
			}
		}
		stage ("build") {
			steps {
				sh 'mvn clean install'
			}
		}
		stage ("test") {
			steps {
				script {
				    def scannerHome = tool 'myscanner';
						withSonarQubeEnv('mysonar') {
						sh "${scannerHome}/bin/sonar-scanner \
						-D sonar.login=admin \
						-D sonar.password=8520 \
						-D sonar.projectKey=applogin \
						-D sonar.exclusions=vendor/**,resources/**,**/*.java \
						-D sonar.host.url=http://3.144.180.238:9000/"
						}
					
               }
			}
		}
		stage ("publish") {
			steps {
				script {
					rtUpload (
						serverId: 'myjfrog',
						spec: '''{
							  "files": [
								{
								  "pattern": "target/*.war",
								  "target": "applogin/"
								}
							  ]
						}''',
						buildName: "${JOB_NAME}",
						buildNumber: "${BUILD_NUMBER}",

					)
				}
			}
		}
		stage ("deploy") {
			steps {
				println "this is deploy"
			}
		}
	}
}

pipeline {
	agent any {
		stage('Build Automation'){
			steps{
				echo 'Building Automation'
				sh './gradlew build --no-daemon'
				archiveArtifacts artifacts: 'dist/trainSchedule.zip'
				}
			}
		}	

		stage('Build Docker image'){
			steps{
				script{
					app = docker.build('swapstyle/trainschedule')
					app.inside {
					sh 'echo $(curl localhost:8080)'
					}
				}
			}
		}	

		stage('Push docker image'){
			steps{
				script{
					docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
					}
				}
			}
		}
	}

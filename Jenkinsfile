pipeline{
	agent myLappyNode
	stages {
		stage('Build Maven App'){
			steps{
				docker-machine ssh default "docker run -it --rm --name mymavenproject -v "/Docker_Shared_FS/Maven_FS/myMavenRepo/myMavenProject":/usr/src/mymaven -w /usr/src/mymaven maven:3.6.1-jdk-8-slim mvn clean install"
			}
		}
		stage('Start myUbuntu Docker container'){
			steps{
				docker-machine ssh default "docker start myUbuntu"
				sleep 60
			}
		}
		stage('Copy Jar to Ubuntu Machine'){
			steps{
				docker-machine ssh default "docker cp /Docker_Shared_FS/Maven_FS/myMavenRepo/myMavenProject/target/myMavenProject-1.0-SNAPSHOT.jar myUbuntu:/tmp"
			}
		}
		stage('Execute the Jar on Ubuntu Machine'){
			steps{
				docker-machine ssh default "docker exec myUbuntu /bin/bash -c \"java -jar /tmp/myMavenProject-1.0-SNAPSHOT.jar\""
			}
		}
	}
}
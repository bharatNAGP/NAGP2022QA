pipeline{
    agent any
    	environment {
		notifyEmail ="bharat.garg@nagarro.com"
	}
    tools{
        maven 'MAVEN'
    }
    stages{
        stage("code checkout"){
            steps{
            bat "echo hello"
            }
        }   
        stage("code build"){
            steps{
            bat "mvn clean"
            }
        }
        stage("unit test"){
            steps{
            bat "mvn test"
            }
        }
        stage("Sonar Analysis"){
            steps{
            withSonarQubeEnv("SONAR_TEST")
                {
		    bat "echo Sonar Run half"
                        bat "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.8.0.2131:sonar"        
                }
            }
        }
        stage("Publish to Artifactory"){
            steps{
                rtMavenDeployer(
                    id: 'deployer',
                    serverId: '123Bharat_Artifactory',
                    releaseRepo: 'nagp.bharat2022',
                    snapshotRepo: 'nagp.bharat2022'
                )
                rtMavenRun(
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: 'deployer'
                    )
                rtPublishBuildInfo(
                    serverId:'123Bharat_Artifactory',
                )
            }        
        }
        
    }
    post{
        success{
            bat "echo success"
            }
        }
}

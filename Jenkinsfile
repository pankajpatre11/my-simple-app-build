pipeline 
{
    agent any

    environment{
	SONAR_TOKEN = "ebe715a7d0fdd4ffb924ae703699a6131009211a"
	GIT_COMMIT_SHORT = sh(
     script: "printf \$(git rev-parse --short ${GIT_COMMIT})",
     returnStdout: true)
        imageName = "pankajpatre11/myapp"
        registryCredentials = "dockerhub"
        registry = "18.208.249.204:8083"
        dockerImage = ''
    }
    options {
       buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
       }
    stages
    {
        stage('Build')
        {
            steps
            {
		    sh script: 'pwd'
                 sh script: 'mvn -f MyAwesomeApp/pom.xml clean package'
            }
         }
    stage('Upload War To Repo') {
      steps {
        script {
          def mavenPom = readMavenPom file: 'MyAwesomeApp/pom.xml'
          def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "maven-snapshots" : "maven-releases"
          nexusArtifactUploader artifacts: [
              [artifactId: 'maven-project',
                classifier: '',
	        file: "target/springbootApp.jar",
                type: 'jar'
               # file: "target/maven-project-${mavenPom.version}.war",
               # type: 'war'
              ]
            ],
            credentialsId: 'nexusid1',
            groupId: 'com.example',
            nexusUrl: '54.226.187.187:8081',
            nexusVersion: 'nexus3',
            protocol: 'http',
            repository: nexusRepoName,
            version: "${mavenPom.version}"
        }
      }
    }	    

	  	    
    
	    
    }
}




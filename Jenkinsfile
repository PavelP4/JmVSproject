pipeline {
    agent any
    stages {
		stage('Restore packages') {
			steps {
				bat 'nuget restore WebAppSimple.sln'
			}
		}  
		stage('Build') {
			steps {
				bat "\"${tool 'MSBuild'}\" ${WORKSPACE}\WebAppSimple.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"				
			}			
		}
    }
}
pipeline {
    agent any
    stages {
		stage('Restore packages') {
			steps {
				bat echo 'Restoring packages...'
				bat 'nuget restore WebAppSimple.sln'
			}
		}  
		stage('Build') {
			steps {
				bat echo 'Building...'
				bat "\"${tool name:'MSBuild', type:'msbuild'}\" WebAppSimple.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"				
			}			
		}
    }
	post {
		success {
			bat echo 'Pipeline Succeeded'
		}
		failure {
			bat echo 'Pipeline Failed'
		}
		unstable {
			bat echo 'Pipeline run marked unstable'
		}
	}
}
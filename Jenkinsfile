pipeline {
    agent any
	environment {
		MSBuild = tool 'MSBuild.exe'msbuild.exe
	}
	options {
		skipDefaultCheckout()
	}
    stages {
		stage('Restore packages') {
			steps {
				echo 'Restoring packages...'
				//bat 'nuget restore WebAppSimple.sln'
			}
		}  
		stage('Build') {
			steps {
				echo 'Building...'
				bat "\"${MSBuild}\" WebAppSimple.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"				
			}			
		}
    }
	/*
	post {
		success {
			echo 'Pipeline Succeeded'
		}
		failure {
			echo 'Pipeline Failed'
		}
		unstable {
			echo 'Pipeline run marked unstable'
		}
	}
	*/
}
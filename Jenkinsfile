pipeline {
    agent any
	environment {
		MSBuild = tool 'MSBuild.exe'
	}
	parameters {
		string(name: 'DevPath', defaultValue: 'D:\Work_Jenkins\WebAppSimpleDev', description: 'DevPath')
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
		stage('Build&Deploy for test') {
			steps {
				echo 'Building...'				
				bat "\"${MSBuild}msbuild\" WebAppSimple.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"								
			}			
		}
		stage('Copy') {
			steps {
				bat "IF EXIST \"${params.DevPath}\" RD /Q /S \"${params.DevPath}\""
				bat "XCOPY \"${WORKSPACE}\WebAppSimple\*\" \"${params.DevPath}\" /s /e /y /i"
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
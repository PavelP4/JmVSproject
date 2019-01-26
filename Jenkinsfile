pipeline {
    agent any
	environment {
		MSBuild = tool 'MSBuild'
		DevPath = 'D:\\Work_Jenkins\\WebAppSimpleDev'
		
	}	
	parameters {
		//string(name: 'DevPath', defaultValue: 'D:\\Work_Jenkins\\WebAppSimpleDev', description: 'DevPath')
		choice(name: 'DEV_PROD', choices: ['DEV', 'PROD'], description: '')
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
				bat "\"${MSBuild}\" WebAppSimple.sln /p:Configuration=Release /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"								
			}			
		}
		stage('Copy DEV') {
			when {                
                equals expected: 'DEV', actual: params.DEV_PROD
            }
			steps {
				bat "IF EXIST \"${env.DevPath}\" RD /Q /S \"${env.DevPath}\""
				bat "XCOPY \"${WORKSPACE}\\WebAppSimple\\*\" \"${env.DevPath}\" /s /e /y /i"
			}
		}
		stage('Copy PROD') {
			when {                
                equals expected: 'PROD', actual: params.DEV_PROD
            }
			steps {
				echo 'Copy prod...'
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
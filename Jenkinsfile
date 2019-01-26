pipeline {
    agent any
	environment {
		MSBuild = tool 'MSBuild'
		TestPath = 'D:\\Work_Jenkins\\WebAppSimplePublish'
		DevPath = 'D:\\Work_Jenkins\\WebAppSimpleDev'
		ProdPath = 'D:\\Work_Jenkins\\WebAppSimpleProd'
		BuildConf = 'Release'
		OPT_DEV = 'DEV'
		OPT_PROD = 'PROD'
	}	
	parameters {
		//string(name: 'DevPath', defaultValue: 'D:\\Work_Jenkins\\WebAppSimpleDev', description: 'DevPath')
		choice(name: 'DEV_PROD', choices: ['DEV', 'PROD'], description: '')
	}
	/*
	options {
		skipDefaultCheckout()
	}
	*/
    stages {
		stage('Restore packages') {
			steps {
				echo 'Restoring packages...'
				bat 'nuget restore WebAppSimple.sln'
			}
		}  
		stage('Build&Deploy for test') {
			steps {
				echo 'Building...'		

				script {
					if (params.DEV_PROD == 'DEV') {
						env.BuildConf = 'Debug'
					} else {
						env.BuildConf = 'Release'
					}
				}
				
				bat "\"${MSBuild}\" WebAppSimple.sln /p:Configuration=${env.BuildConf} /p:Platform=\"Any CPU\" /p:ProductVersion=1.0.0.${env.BUILD_NUMBER} /P:DeployOnBuild=True /P:PublishProfile=WASProfile"								
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
				fileOperations([
					folderDeleteOperation(env.ProdPath), 
					folderCopyOperation(destinationFolderPath: env.ProdPath, sourceFolderPath: env.TestPath)
					])				
			}
		}
    }
	
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
	
}
pipeline {
    agent any
    stages {
		stage('Restore packages') {
			steps {
				bat 'nuget restore WebAppSimple.sln'
			}
		}            
    }
}
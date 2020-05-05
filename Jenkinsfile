pipeline {
	agent any
	
	stages {
		stage('Copy artifact') { 
			steps {
				copyArtifacts filter: 'Class2', fingerprintArtifacts: true, projectName: 'Class2', selector: lastSuccessful()
			}
		}
		stage('Deliver') {
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/$BRANCH/hosts.ini', playbook: 'playbook.yml'
			}
		}
	}
}
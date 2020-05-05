pipeline {
	agent any
	
	stages {
		stage('Copy artifact') { 
			steps {
				copyArtifacts filter: 'Class2', fingerprintArtifacts: true, projectName: 'Class2', selector: lastSuccessful()
			}
		}
		stage('Deliver to prod') {
			when {
				branch 'master'
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/production/hosts.ini', playbook: 'playbook.yml'
			}
		}
		stage('Deliver to staging') {
			when {
				branch 'staging'
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/staging/hosts.ini', playbook: 'playbook.yml'
			}
		}
	}
}
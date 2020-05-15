pipeline {
	agent any
	
	parameters {
		choice(name: 'TARGET_ENV', choices: ['staging', 'production'], description: 'Please choose an environment')
	}
	
	stages {
		stage('Copy artifacts') { 
			steps {
				copyArtifacts filter: 'ruby_deploy.tar.gz', fingerprintArtifacts: true, projectName: 'finalTest', selector: lastSuccessful()
			}
		}
		stage('Deliver') {
			steps {
				ansiblePlaybook become: true, 
				disableHostKeyChecking: true,
				credentialsId: 'vagrant-ssh', 
				inventory: "environments/${params.TARGET_ENV}/hosts.ini", 
				playbook: 'playbook.yml'
			}
		}
	}
}
pipeline {
	agent any
	
	parameters {
		choice(name: 'TARGET_ENV', choices: ['staging', 'production', 'ec2'], description: 'Please choose an environment')
	}
	
	stages {
		stage('Copy artifact') { 
			steps {
				copyArtifacts filter: 'GoServerWithTests', fingerprintArtifacts: true, projectName: 'GoServerWithTests', selector: lastSuccessful()
			}
		}
		stage('Deliver') {
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/${params.TARGET_ENV}/hosts.ini', playbook: 'playbook.yml'
			}
		}
		stage('Test') {
			steps {
				sh 'docker run -v $HOME/workspace/Deploy/environments/${params.TARGET_ENV}:/etc/newman -t postman/newman run "https://www.getpostman.com/collections/e512d3d2e594071a5cfa" -e postman_environment.json'
			}
		}
	}
}
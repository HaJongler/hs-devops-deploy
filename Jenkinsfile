pipeline {
	agent any
	
	parameters {
		gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
	}
	
	stages {
		stage('Copy artifact') { 
			steps {
				copyArtifacts filter: 'GoServerWithTests', fingerprintArtifacts: true, projectName: 'GoServerWithTests', selector: lastSuccessful()
			}
		}
		stage('Deliver to prod') {
			when {
				expression {
					params.BRANCH == 'master'
				}
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/production/hosts.ini', playbook: 'playbook.yml'
			}
		}
		stage('Test E2E prod') {
			when {
				expression {
					params.BRANCH == 'master'
				}
			}
			steps {
				sh 'docker run -v $HOME/environments/production:/etc/newman -t postman/newman run "https://www.getpostman.com/collections/e512d3d2e594071a5cfa" -e postman_environment.json'
			}
		}
		stage('Deliver to staging') {
			when {
				expression {
					params.BRANCH == 'staging'
				}
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/staging/hosts.ini', playbook: 'playbook.yml'
			}
		}
		stage('Test E2E staging') {
			when {
				expression {
					params.BRANCH == 'staging'
				}
			}
			steps {
				sh 'docker run -v $HOME/environments/staging:/etc/newman -t postman/newman run "https://www.getpostman.com/collections/e512d3d2e594071a5cfa" -e postman_environment.json'
			}
		}
	}
}
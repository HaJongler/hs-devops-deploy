pipeline {
	agent any
	
	parameters {
		gitParameter branchFilter: 'origin/(.*)', defaultValue: 'master', name: 'BRANCH', type: 'PT_BRANCH'
	}
	
	stages {
		stage('Copy artifact') { 
			steps {
				copyArtifacts filter: 'Class2', fingerprintArtifacts: true, projectName: 'Class2', selector: lastSuccessful()
			}
		}
		stage('Deliver to prod') {
			when {
				expression {
					${params.BRANCH} == 'master'
				}
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/production/hosts.ini', playbook: 'playbook.yml'
			}
		}
		stage('Deliver to staging') {
			when {
				expression {
					${params.BRANCH} == 'staging'
				}
			}
			steps {
				ansiblePlaybook become: true, credentialsId: 'vagrant-ssh', inventory: 'environments/staging/hosts.ini', playbook: 'playbook.yml'
			}
		}
	}
}
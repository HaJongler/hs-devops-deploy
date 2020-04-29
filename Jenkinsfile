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
            sshagent(['toobox-vagrant-key']) {
                sh 'ansible-playbook -i hosts.ini'
            }
         }
      }
   }
}
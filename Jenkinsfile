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
            sshagent(['vagrant-ssh']) {
                sh 'scp -o StrictHostKeyChecking=no Class2 'vagrant@10.10.50.3:~''
            }
         }
      }
   }
}
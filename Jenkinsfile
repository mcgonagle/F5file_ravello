node {
   stage('Preparation') { 
      // Get some code from a GitHub repository
      git 'https://github.com/mcgonagle/F5file_ravello.git'
   }
   stage('Testing') {
      //Run the tests
      sh "/usr/bin/ansible-lint F5file_build.yml"
      //sh "/usr/bin/ansible-review F5file_build.yml"
   }
   stage('Build') {
       //Ansible Playbook
       ansiblePlaybook(
         colorized: true, 
         inventory: 'hosts.ini', 
         playbook: 'F5file_build.yml', 
         sudoUser: null,
         extraVars: [
            username: 'admin',
            password: [value: 'admin', hidden: true],
            hosts: 'bigip_hosts'
         ])
      //chatops slack message that ansible run has completed
      slackSend( 
         channel: '#general', 
         color: 'good', 
         message: 'F5file Build Ran Successfully', 
         teamDomain: 'uniopsteam', 
         token: 'zkMRYtEXCEG3Q2FlUsS2Hjjv'
         )
   }
   stage('Approval') {
      //Gate the process and require approval
      input 'Proceed?'
   }
   stage('Re-Enable') {
       //Ansible Playbook
       ansiblePlaybook(
         colorized: true, 
         inventory: 'hosts.ini', 
         playbook: 'F5file_enable.yml', 
         sudoUser: null,
         extraVars: [
            username: 'admin',
            password: [value: 'admin', hidden: true],
            hosts: 'bigip_hosts'
         ])
      //chatops slack message that ansible run has completed
      slackSend( 
         channel: '#general', 
         color: 'good', 
         message: 'F5file Node Re-enabled', 
         teamDomain: 'uniopsteam', 
         token: 'zkMRYtEXCEG3Q2FlUsS2Hjjv'
         )
   }
}

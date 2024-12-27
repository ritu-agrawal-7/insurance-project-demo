node {
    stage('checkout'){
        git 'https://github.com/ritu-agrawal-7/insurance-project-demo.git'
    }
    stage('maven build'){
        sh 'mvn clean package'    
    }
    stage ('containerize'){
    //    sh 'docker build -t rituagrawal18/insure-me:1.0 .'
    }
    stage('Release'){
        withCredentials([string(credentialsId: 'dockerHubPass', variable: 'dockerHubPwd')]) {
      //                   sh "docker login -u rituagrawal18 -p ${dockerHubPwd}"
        //                 sh 'docker push rituagrawal18/insure-me:1.0'
        }
    }
    stage('Deploy to test'){
       ansiblePlaybook become: true, credentialsId: 'ansible-testserverkey', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml', vaultTmpPath: ''
    }
    stage('checkout regression test code'){
        git 'https://github.com/ritu-agrawal-7/insureme.testscripts.git'
    }
    stage('build test scripts'){
        sh 'mvn clean package assembly:single'
    }
    stage ('execute selenium scripts'){
        sh 'java -jar target/insureme.testscripts-0.0.1-SNAPSHOT-jar-with-dependencies.jar'
    }
    stage('checkout'){
        git 'https://github.com/ritu-agrawal-7/insurance-project-demo.git'
    }
    stage('Deploy to prod'){
       ansiblePlaybook become: true, credentialsId: 'ansible-testserverkey', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-prod-server.yml', vaultTmpPath: ''
    }
}

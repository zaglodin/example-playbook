pipeline {
    agent {
        label "ansible_docker"
    }

    stages {
        stage('Git'){
            steps {
                git 'https://github.com/zaglodin/example-playbook.git'
            }
        }    
        stage('Secret'){
            steps {
                sh "ansible-vault decrypt secret --vault-password-file vault_pass"
                sh "mkdir -p ~/.ssh/"
                sh "mv ./secret ~/.ssh/id_rsa"
                sh "chmod 400 ~/.ssh/id_rsa"
                sh "echo -e 'Host github.com\\n StrictHostKeyChecking no\\n UserKnownHostsFile=/dev/null' > ~/.ssh/config"
            }
        }    
        stage('Playbook'){
            steps {      
                sh "ansible-galaxy install -r requirements.yml -p roles"
                sh "ansible-playbook site.yml -i inventory/prod.yml"
            }    
        }
    }
}    

pipeline {
    agent { docker { image 'willhallonline/ansible:2.9-alpine' } }

    environment {
        ANSIBLE_INVENTORY = credentials('DB_ANSIBLE_INVENTORY')
        SSH_PRIVATE_KEY   = credentials('DB_SSH_PRIVATE_KEY')
        POSTGRES_PASSWORD = credentials('DB_POSTGRES_PASSWORD')
    }

    stages {
        stage('deploy') {
            steps {
                sh '''
                eval $(ssh-agent -s)
                echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
                echo "$ANSIBLE_INVENTORY" >inventory
                ansible-playbook \
                    --inventory "inventory" \
                    --ssh-common-args '-o StrictHostKeyChecking=no' \
                    --extra-vars "{\"POSTGRES_PASSWORD\": \"$POSTGRES_PASSWORD\"}" \
                    ./playbook.yml
                '''
            }
        }
    }
}

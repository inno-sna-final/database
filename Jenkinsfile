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
                echo "$SSH_PRIVATE_KEY" >key
                echo "$ANSIBLE_INVENTORY" >inventory
                chmod 0600 key
                ansible-playbook \
                    --inventory "inventory" \
                    --ssh-common-args '-o StrictHostKeyChecking=no' \
                    --extra-vars "{\"POSTGRES_PASSWORD\": \"$POSTGRES_PASSWORD\", \"SSH_PRIVATE_KEY_FILE\": \"key\"}" \
                    ./playbook.yml
                '''
            }
        }
    }
}

pipeline {
    agent { docker { image 'willhallonline/ansible:2.9-alpine' } }

    environment {
        ANSIBLE_INVENTORY = credentials('DB_ANSIBLE_INVENTORY_FILE')
        SSH_PRIVATE_KEY   = credentials('DB_SSH_PRIVATE_KEY_FILE')
        POSTGRES_PASSWORD = credentials('DB_POSTGRES_PASSWORD')
    }

    stages {
        stage('deploy') {
            steps {
                sh '''
                eval $(ssh-agent -s)
                cat "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
                ansible-playbook \
                    --inventory "$ANSIBLE_INVENTORY" \
                    --ssh-common-args '-o StrictHostKeyChecking=no' \
                    --extra-vars "{\"POSTGRES_PASSWORD\": \"$POSTGRES_PASSWORD\"}" \
                    ./playbook.yml
                '''
            }
        }
    }
}

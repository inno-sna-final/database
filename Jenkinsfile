pipeline {
    agent { docker { image 'ansible/ansible:default' } }

    environment {
        ANSIBLE_INVENTORY = credentials('DB_ANSIBLE_INVENTORY')
        SSH_PRIVATE_KEY   = credentials('DB_SSH_PRIVATE_KEY')
        POSTGRES_PASSWORD = credentials('DB_POSTGRES_PASSWORD')
    }

    stages {
        stage('deploy') {
            steps {
                eval $(ssh-agent -s)
                echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
                ansible-playbook \
                    --inventory "$ANSIBLE_INVENTORY" \
                    --ssh-common-args '-o StrictHostKeyChecking=no' \
                    --extra-vars "{\"POSTGRES_PASSWORD\": \"$POSTGRES_PASSWORD\"}" \
                    ./playbook.yml
            }
        }
    }
}

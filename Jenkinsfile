node {
    stage 'build'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" build'
    stage 'test'
    sh 'echo "Starting containers..."'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" up -d --remove-orphans'
    sh 'docker run --rm --interactive --tty -v "/var/lib/jenkins/workspace/Stock Service Pipeline@script":/app composer install'
    sh 'echo "Starting tests"'
    sh 'sleep 400 # Wait for containers to be ready'
    sh './vendor/bin/phpunit -c tests/unit/phpunit.xml tests/unit'
    sh 'echo "Stopping containers..."'
    sh 'docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" stop'
    sh 'echo "Removing containers..."'
    sh 'docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" rm -f'
    stage 'deploy'
    sh 'echo "Starting containers..."'
    sh 'docker-compose -f docker-compose-deploy.yml up -d --force-recreate'
}
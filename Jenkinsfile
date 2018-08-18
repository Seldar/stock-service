node {
    stage 'build'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" build'
    stage 'test'
    sh 'echo "Starting containers..."'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" up -d --remove-orphans'
    sh 'sudo docker run --rm -v "/var/lib/jenkins/workspace/Stock Service Pipeline@script":/app composer install'
    sh 'echo "Starting tests"'
    sh '"../Stock Service Pipeline@script/vendor/bin/phpunit" -c tests/unit/phpunit.xml tests/unit'
    sh 'echo "Stopping containers..."'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" stop'
    sh 'echo "Removing containers..."'
    sh 'sudo docker-compose -f "../Stock Service Pipeline@script/docker-compose.yml" rm -f'
    stage 'deploy'
    sh 'echo "Starting containers..."'
    sh 'sudo docker-compose -f docker-compose-deploy.yml up -d --force-recreate'
}
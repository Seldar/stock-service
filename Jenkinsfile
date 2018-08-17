node {
    stage 'build'
        sh 'docker-compose -f docker-compose.yml build
    stage 'test'
        sh 'echo "Starting containers..."'
        sh 'docker-compose -f docker-compose.yml up -d --remove-orphans'
        sh 'echo "Starting tests"'
        sh 'sleep 400 # Wait for containers to be ready'
        sh 'tools/ci/tests.bash'
        sh 'echo "Stopping containers..."'
        sh 'docker-compose -f docker-compose.yml stop'
        sh 'echo "Removing containers..."'
        sh 'docker-compose -f docker-compose.yml rm -f'
    stage 'deploy'
        sh 'echo "Starting containers..."'
        sh 'docker-compose -f docker-compose-deploy.yml up -d --force-recreate'
}
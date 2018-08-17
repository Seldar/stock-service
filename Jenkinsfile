node {
    stage 'build'
    withEnv(['DOCKER_REGISTRY=192.168.218.61:5001']) {

          sh 'docker-compose -f docker-compose-ci.yml build'

    }
    stage 'test'
    withEnv(['DOCKER_REGISTRY=192.168.218.61:5001']) {
        sh 'echo "Starting containers..."'
        sh 'docker-compose -f docker-compose-ci.yml up -d --remove-orphans'
        sh 'echo "Starting tests"'
        sh 'sleep 400 # Wait for containers to be ready'
        sh 'tools/ci/tests.bash'
        sh 'echo "Stopping containers..."'
        sh 'docker-compose -f docker-compose-ci.yml stop'
        sh 'echo "Removing containers..."'
        sh 'docker-compose -f docker-compose-ci.yml rm -f'
    }

    stage 'deploy'
}
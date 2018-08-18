node {
    stage ('build') {
        checkout([$class: 'GitSCM',
                branches: [[name: '*/master']],
                doGenerateSubmoduleConfigurations: false,
                submoduleCfg: [],
                userRemoteConfigs: [[url: 'https://github.com/Seldar/stock-service.git']]])
        sh 'sudo docker-compose -f docker-compose.yml build'
    }
    stage ('test') {
        sh 'sudo docker run --rm -v "/var/lib/jenkins/workspace/Stock Service Pipeline":/app composer install'
        sh 'echo "Starting containers..."'
        sh 'sudo docker-compose -f docker-compose.yml up -d --remove-orphans'
        sh 'echo "Starting tests"'
        sh 'sudo docker exec -i stockservicepipeline_php_1 /var/www/vendor/bin/phpunit -c ./tests/unit/phpunit.xml tests/unit'
        sh 'echo "Stopping containers..."'
        sh 'sudo docker-compose -f docker-compose.yml stop'
        sh 'echo "Removing containers..."'
        sh 'sudo docker-compose -f docker-compose.yml rm -f'
    }
    stage ('deploy') {
        sh 'echo "Starting containers..."'
        sh 'sudo docker-compose -f docker-compose-deploy.yml up -d --force-recreate'
        sh 'sudo docker exec -i stockservicepipeline_php_1 mysql < database.sql'
    }
}
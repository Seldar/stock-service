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
        sh 'sudo docker-compose -f docker-compose.yml up -d'
        sh 'echo "Starting tests"'
        sh 'sudo docker exec -i service-stock-php /var/www/vendor/bin/phpunit -c ./tests/unit/phpunit.xml tests/unit'
        sh 'echo "Stopping containers..."'
        sh 'sudo docker-compose -f docker-compose.yml stop'
        sh 'echo "Removing containers..."'
        sh 'sudo docker-compose -f docker-compose.yml rm -f'
    }
    stage ('deploy') {
        sh 'echo "Starting containers..."'
        sh 'sudo docker stack deploy -c docker-compose-deploy.yml stock-service'
        sh 'sleep 3'
        sh 'sudo docker exec -i service-stock-mysql mysql -uroot -proot < database.sql'
    }
}
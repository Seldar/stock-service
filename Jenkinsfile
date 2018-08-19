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
        sh 'cat ~/wf3rg.txt | sudo docker login --username vulukut --password-stdin'
        sh 'echo "Starting containers..."'
        sh 'sudo docker build -t "vulukut/stock-service-php" -f docker/php/Dockerfile .'
        sh 'sudo docker push "vulukut/stock-service-php"'
        sh 'rsync -avrt --delete --rsh='ssh -p 22' ./ root@172.31.42.196:/home/ubuntu/'
        sh 'rsync -avrt --delete --rsh='ssh -p 22' ./ root@172.31.39.151:/home/ubuntu/'
        sh 'sudo docker stack deploy -c docker-compose-deploy.yml --with-registry-auth stock-service'
    }
}
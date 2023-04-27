node {
    checkout scm

    // build
    stage("Build"){
        docker.image('shippingdocker/php-composer:7.4').inside('-u root') {
            sh 'rm composer.lock'
            sh 'composer install'
        }
    }


    // Testing
    stage("Test"){
        docker.image('ubuntu').inside('-u root') {
        sh 'echo "Ini adalah test"'
        }
    }

    // Deploy
    stage("Deploy"){
        docker.image('agung3wi/alpine-rsync:1.1').inside('-u root') {
            sshagent (credentials: ['$SSH_KEY_NAME']) {
                sh 'mkdir -p ~/.ssh'
                sh 'ssh-keyscan -H "$HOSTNAME" > ~/.ssh/known_hosts'
                sh "rsync -rav --delete ./ ubuntu@$HOSTNAME:/home/ubuntu/$HOSTNAME/ --exclude=.env --exclude=storage --exclude=.git"
            }
        }
    }

}
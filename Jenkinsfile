node {

    environment {
        POSTGRES_DB_PASSWORD = 'test-password'
    }

    stage('checkout and clean') {
        checkout scm
    }

    stage('read secrets from vault') {
        // define the secrets and the env variables
        // engine version can be defined on secret, job, folder or global.
        // the default is engine version 2 unless otherwise specified globally.
        def secrets = [
            [path: '/secret/rds/ippon/postgres', engineVersion: 1, secretValues: [
                [envVar: 'db_username', vaultKey: 'username'],
                [envVar: 'db_password', vaultKey: 'password']]]
        ]

        // optional configuration, if you do not provide this the next higher configuration
        // (e.g. folder or global) will be used
        def configuration = [vaultUrl: 'https://vault.qomplx.com:8200',
                             vaultCredentialId: 'jenkins-vault-approle',
                             engineVersion: 1]
        // inside this block your credentials will be available as env variables
        withVault([configuration: configuration, vaultSecrets: secrets]) {
            sh 'echo $db_username'
            sh 'echo $db_password'  

            def filename = 'db-secret.yml'
            //def filenamenew = 'db-secret-new.yml'
            def yml = readYaml file: filename

            // Change something in the file
            yml.data.secret = env.db_password

            sh "rm $filename"
            writeYaml file: filename, data: yml          
        }
    }

    stage('change yaml config'){

        // sh 'echo db password from env is: ${POSTGRES_DB_PASSWORD}'

        // def filename = 'db-secret.yml'
        // def filenamenew = 'db-secret-new.yml'
        // def yml = readYaml file: filename

        // // Change something in the file
        // yml.data.secret = env.db_password

        // sh "rm $filename"
        // writeYaml file: filenamenew, data: yml
    }

}
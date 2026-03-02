stage('Run tests') {
    steps {
        script {
            if (params.TEST_TYPE == 'API') {
                sh "mvn clean test -PAPI"
            } else if (params.TEST_TYPE == 'UI') {
                withCredentials([
                    string(credentialsId: 'VALID_LOGIN', variable: 'VALID_LOGIN'),
                    string(credentialsId: 'INVALID_LOGIN', variable: 'INVALID_LOGIN'),
                    string(credentialsId: 'PASSWORD', variable: 'PASSWORD'),
                    string(credentialsId: 'BASE_URL', variable: 'BASE_URL'),
                    string(credentialsId: 'FIRST_NAME', variable: 'FIRST_NAME'),
                    string(credentialsId: 'LAST_NAME', variable: 'LAST_NAME'),
                    string(credentialsId: 'POSTAL_CODE', variable: 'POSTAL_CODE')
                ]) {
                    sh """
                    echo "VALID_LOGIN=${VALID_LOGIN}" > .env
                    echo "INVALID_LOGIN=${INVALID_LOGIN}" >> .env
                    echo "PASSWORD=${PASSWORD}" >> .env
                    echo "BASE_URL=${BASE_URL}" >> .env
                    echo "FIRST_NAME=${FIRST_NAME}" >> .env
                    echo "LAST_NAME=${LAST_NAME}" >> .env
                    echo "POSTAL_CODE=${POSTAL_CODE}" >> .env

                    mvn clean test -PUI
                    """
                }
            }
        }
    }
    post {
        always {
            junit 'target/surefire-reports/TEST-*.xml'
            archiveArtifacts(artifacts: 'target/allure-results/**', allowEmptyArchive: true)
            allure(results: [[path: 'target/allure-results']])
        }
    }
}

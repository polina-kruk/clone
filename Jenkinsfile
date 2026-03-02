pipeline {
    agent any

    parameters {
        choice(name: 'TEST_TYPE', choices: ['API', 'UI'], description: 'Select test type')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Run tests') {
            steps {
                script {
                    if (params.TEST_TYPE == 'API') {
                        sh "mvn clean test -PAPI"
                    } else {
                        withCredentials([
                            string(credentialsId: 'VALID_LOGIN', variable: 'VALID_LOGIN'),
                            string(credentialsId: 'PASSWORD', variable: 'PASSWORD')
                        ]) {
                            sh """
                            echo "VALID_LOGIN=${VALID_LOGIN}" > .env
                            echo "PASSWORD=${PASSWORD}" >> .env
                            mvn clean test -PUI
                            echo "SOMETHING TO OUTPUT IN CONSOLE"
                            """
                        }
                    }
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

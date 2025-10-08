pipeline {
    agent any

    stages {
        stage('Install Dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'pytest test_app.py'
            }
        }

        stage('Deploy') {
            when {
                anyOf{
                    branch 'main'
                    branch pattern: "release/.*", comparator: "REGEXP"
                }
            }
            steps {
                echo "Simulating deploy from branch ${env.BRANCH_NAME}"
            }
        }
    }

    post {
        success {
            script {
                def payload = [
                    content:"Build SUCCESS on ${env.BRANCH_NAME} \n URL: ${env.BUILD_URL}"
                ]
            httpRequest(
                httpMode: 'POST',
                contentType: 'APPLICATION_JSON',
                requestBody: groovy.json.JsonOutput.toJson(payload),
                url: 'https://discordapp.com/api/webhooks/1425418498246705235/HT2o_T37WyxaOnJLH5-A0MsGWDKiclwoCSokqDI4w3VnVSvhdBYMa6s4sQARyYXr5wd5'
            )
        }
    }

        failure{
            script {
                def payload = [
                    content: "Build FAILED on `${env. BRANCH_NAME}` \n URL: ${env.BUILD_URL}"
                ]
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON'
                    requestBody: groovy.json.JsonOutput.toJson(payload),
                    url: 'https://discordapp.com/api/webhooks/1425418498246705235/HT2o_T37WyxaOnJLH5-A0MsGWDKiclwoCSokqDI4w3VnVSvhdBYMa6s4sQARyYXr5wd5'
                )
            }
        }
    }
}


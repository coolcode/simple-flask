pipeline {
    agent {
        label "ec2"
    }
    environment {
        ORG         = 'blocktest'
        APP_NAME    = 'simple-flask'
    }
    stages {
        stage('Docker Deploy') {
            agent {
                label "ec2"
            }
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying....'
                sh "pwd"
                sh "curl https://api.ipify.org"
                withPythonEnv('python3') {
                    sh "python3 -V"
                    sh "pkill gunicorn || echo 'gunicorn was not running'"
                    sh "sleep 1"
                    sh "pip3 install -r requirements.txt"
                    sh "JENKINS_NODE_COOKIE=dontKillMe nohup gunicorn --workers 3 -t 30 --graceful-timeout 60 --bind :8000 -m 007 application:app > gunicorn.log 2>&1 &"
                }
                sh "tail gunicorn.log"
            }
        }
    }
}
pipeline {
    agent any

    environment {
        EC2_USER = "ubuntu"
        EC2_HOST = "13.126.223.241"
        APP_DIR  = "/home/ubuntu/app"
        APP_NAME = "hello-world.war"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/Harshitha1518/hello-world-war-java.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['ec2-ssh-key']) {
                    sh """
                    scp -o StrictHostKeyChecking=no \
                    target/*.war \
                    ${EC2_USER}@${EC2_HOST}:${APP_DIR}/${APP_NAME}

                    ssh -o StrictHostKeyChecking=no ${EC2_USER}@${EC2_HOST} '
                        pkill -f ${APP_NAME} || true
                        nohup java -jar ${APP_DIR}/${APP_NAME} > app.log 2>&1 &
                    '
                    """
                }
            }
        }
    }
}

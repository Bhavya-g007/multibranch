pipeline {
    agent {label 'agent1'}

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/firstcheck-organization/jenkins.git'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    cd javaapp-pipeline
                    mvn clean test
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                    cd javaapp-pipeline
                    mvn clean package -DskipTests
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    cd javaapp-pipeline/target
                    if pgrep -f "java -jar java-sample-21-1.0.0.jar" > /dev/null; then
                        pkill -f "java -jar java-sample-21-1.0.0.jar"
                        echo "App was running and has been killed."
                    else
                        echo "App is not running."
                    fi
                    JENKINS_NODE_COOKIE=dontKillMe nohup java -jar java-sample-21-1.0.0.jar > app.log 2>&1 &
                '''
            }
        }

    }

}

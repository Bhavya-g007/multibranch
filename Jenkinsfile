pipeline {
    agent any

    stages {
        stage ('checkout') {
            steps {
               echo "Running on branch: ${env.BRANCH_NAME}"
            }
        }

        stage ('unit tests') {
            steps {
                sh 'mvn clean test'
            }
        }

        stage ('build') {
            when {branch 'main'}
            steps {
                sh 'mvn clean package'
            }
        }

        stage ('deploy') {
            when {branch 'main'}
            steps {
                sh '''
                    if pgrep -f "java -jar java-sample-21-1.0.0.jar" > /dev/null; then
                        pkill -f "java -jar java-sample-21-1.0.0.jar"
                        echo "App was running and has been killed."
                    else
                        echo "App is not running."
                    fi
                    cd target
                    JENKINS_NODE_COOKIE=dontKillMe nohup java -jar java-sample-21-1.0.0.jar > app.log 2>&1 &
                '''
            }
        }
    }
}

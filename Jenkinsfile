pipeline {
    agent any

    tools {
        maven 'maven3'
        jdk 'jdk17'
    }

    stages {

        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Run Application') {
            steps {
                sh '''
                echo "Finding JAR..."
                JAR=$(ls target/*.jar | head -n 1)

                if [ -z "$JAR" ]; then
                    echo "JAR not found!"
                    exit 1
                fi

                echo "Running $JAR"
                timeout 15 java -jar $JAR
                '''
            }
        }
    }

    post {
        success {
            echo 'Build Successful'
        }
        failure {
            echo 'Build Failed'
        }
        always {
            echo 'Pipeline Finished'
        }
    }
}

pipeline{
    agent any
    stages{
        stage ('Build Backend') {           
            steps{
                bat 'mvn clean package -DskipTests=true'
            }
        }
        stage ('Unit Test') {
            steps{
                bat 'mvn test'
            }
        }
        stage ('Deploy Backend') {
            steps{
                deploy adapters: [tomcat8(credentialsId: 'login_tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks-backend', war: 'target/tasks-backend.war'
            }
        }
        stage ('API Test') {
            steps {
                dir('api-test') {
                    git credentialsId: 'login_git_hub', url: 'https://github.com/juniorCarvalho95/tasks-api-test.git'
                    bat 'mvn test'
                }
            }
        }
        stage ('Deploy Frontend') {
            steps {
                dir('frontend') {
                    git credentialsId: 'login_git_hub', url: 'https://github.com/juniorCarvalho95/tasks-frontend.git'
                    bat 'mvn clean package'
                    deploy adapters: [tomcat8(credentialsId: 'login_tomcat', path: '', url: 'http://localhost:8001/')], contextPath: 'tasks', war: 'target/tasks.war'
                }
            }
        }
        stage ('Function Test') {
            steps {
                dir('function-test') {
                    git credentialsId: 'login_git_hub', url: 'https://github.com/juniorCarvalho95/task-function-test.git'
                    bat 'mvn test'
                }
            }
        }

        stage('Deploy Prod'){
            steps{
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }
    }
    post{
        always{
            junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml, api-test/target/surefire-reports/*.xml, function-test/target/surefire-reports/*.xml'
        }
    }
}
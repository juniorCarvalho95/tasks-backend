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
                git credentialsId: 'login_git_hub', url: 'https://github.com/juniorCarvalho95/tasks-api-test.git'
                bat 'mvn test'
            }
        }

    }
}
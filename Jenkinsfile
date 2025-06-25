pipeline {
    agent any

    environment {
        BACKEND_DIR = 'backend'
        STATIC_DIR = 'backend/src/main/resources/static'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/DuVanSang/Lib_mng.git'
            }
        }

        stage('Build ReactJS') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Copy React build to Spring Boot') {
            steps {
                sh 'rm -rf ${STATIC_DIR}/*'
                sh 'mkdir -p ${STATIC_DIR}'
                sh 'cp -r build/* ${STATIC_DIR}/'
            }
        }

        stage('Build Spring Boot') {
            steps {
                dir("${env.BACKEND_DIR}") {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Run JAR') {
            steps {
                dir("${env.BACKEND_DIR}") {
                    sh 'java -jar target/*.jar'
                }
            }
        }
    }
}

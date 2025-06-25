pipeline {
    agent any

    environment {
        BACKEND_DIR = 'backend'
        STATIC_DIR = 'backend\\src\\main\\resources\\static'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/DuVanSang/Lib_mng.git'
            }
        }

        stage('Build ReactJS') {
            steps {
                bat 'npm install'
                bat 'npm run build'
            }
        }

        stage('Copy React build to Spring Boot') {
            steps {
                bat 'if exist "%STATIC_DIR%" rmdir /s /q "%STATIC_DIR%"'
                bat 'mkdir "%STATIC_DIR%"'
                bat 'xcopy /E /Y /I build\\* "%STATIC_DIR%\\"'
            }
        }

        stage('Build Spring Boot') {
            steps {
                dir("${env.BACKEND_DIR}") {
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Run JAR') {
            steps {
                dir("${env.BACKEND_DIR}") {
                    bat 'java -jar target\\*.jar'
                }
            }
        }
    }
}

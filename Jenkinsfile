pipeline {
    agent any

    environment {
        BACKEND_DIR = 'backend'
        STATIC_DIR = 'backend\\src\\main\\resources\\static'
        DEPLOY_DIR = 'D:\\Deploy\\lib_manage'
        TOMCAT_WEBAPPS = 'C:\\Tomcat\\webapps'
    }

    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/DuVanSang/Lib_mng.git'
            }
        }

        stage('Build ReactJS') {
            steps {
                bat 'npm install --legacy-peer-deps'
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

        stage ('Publish') {
            steps {
                echo 'Publishing ReactJS to D:\\Deploy\\lib_manage'
                bat 'xcopy "%WORKSPACE%" "D:\\Deploy\\lib_manage" /E /Y /I /R'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                echo 'Deploying to Tomcat...'
                dir("${env.BACKEND_DIR}\\target") {
                    bat 'copy *.jar "C:\\Tomcat\\webapps\\libmanage.jar" /Y'
                }
            }
        }
        bat 'C:\\Tomcat\\bin\\shutdown.bat'
bat 'timeout /t 2'
bat 'C:\\Tomcat\\bin\\startup.bat'

    }
}

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'php --version'
                sh 'composer install'
                sh 'mv .env.example .env'
                sh 'php artisan key:generate'
            }
        }
        stage('test') {
            steps {
                sh 'vendor/bin/phpunit'
            }
        }
        stage('SonarQube analysis'){
            steps {
                script { 
                    def scanner = tool 'RSScanner';    
                    withSonarQubeEnv('SvenRickSonarqube'){
                        sh "${scanner}/bin/sonar-scanner"
                    }
                    sh 'echo SecLab is slow, so we have to wait.'
                    def time = '11'
                    sleep time.toInteger()
                }
            }
        }
        stage("Quality Gate 1"){
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}

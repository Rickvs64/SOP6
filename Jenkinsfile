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
            def scanner = tool 'SonarQube Scanner 2.8';
            withSonarQubeEnv('My SonarQube'){
                sh "${scanner}/bin/sonar-scanner"
            }
        }
        stage("Quality Gate 1"){
            steps {
                waitForQualityGate abortPipeline: true
            }
        }
    }
}

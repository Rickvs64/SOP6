pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh "docker build -t laravel-app:B${BUILD_NUMBER} -f Dockerfile ."
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

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh "docker build -t laravel-app:B${BUILD_NUMBER} -f Dockerfile ."
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
        stage("Deploy") {
          steps {
             sh "rm -rf storage/logs/*" // Remove sonarqube files
             sh "rm -rf storage/framework/views/*"
             sh "rm -rf storage/framework/sessions/*"
             sh "chmod -R 777 storage" // Give full access to storage folder
             sshPublisher(publishers: [sshPublisherDesc(configName: 'SvenRickSOP', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'docker-compose -f /docker/docker-compose.yml up -d --force-recreate', execTimeout: 12000000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '**/*')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
          }
       }
    }
}

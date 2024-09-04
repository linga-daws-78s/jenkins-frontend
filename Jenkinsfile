pipeline {
    agent {
            label 'AGENT-1'
         }
    options {
        // Timeout counter starts AFTER agent is allocated
        timeout(time: 30, unit: 'MINUTES')
       //  disableConcurrentBuilds()
         ansiColor('xterm')
    }
    //  parameters {
    //    choice(name: 'action', choices: ['Apply', 'Destroy'], description: 'Pick something')
    //  }
    environment {
        def appVersion = '' //variable declaration here.
        nexusURl = '3.84.31.186:8081'

 
    }
    stages {
        stage('read the version') {
            steps {
                script {
                def packageJson = readJSON file: 'package.json'
                appVersion = packageJson.version
                echo "application version: $appVersion"
            }
            }
        }

         stage('Build') { 
            steps {
                sh """
                  zip -q -r frontend-${appVersion}.zip * -x Jenkinsfile -x frontend-${appVersion}.zip
                  ls -ltr
                  
                """ 
            }
        }
        stage('Nexus Artifact Uploader') { 
            steps {
                script {

                    nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: "${nexusUrl}",
                            groupId: 'com.expense',
                            version: "${appVersion}",
                            repository: "frontend",
                            credentialsId: 'nexus-auth',
                            artifacts: [
                                [artifactId: "frontend",
                                classifier: '',
                                file: "frontend-" + "${appVersion}" + '.zip',
                                type: 'zip']
                            ]
                        )

                }
            }
        }
        //  stage('Deploy') { 
        //      steps {
        //         script {
        //             def params = [
        //                 string(name: 'appVersion', value: "${appVersion}")
        //             ]
        //             build job: 'frontend-deploy', parameters: params, wait: false
        //         }   
        //     }
        //  }

    }
     post { 
        always { 
            deleteDir()
            echo 'I will always say Hello Always!'
        }
        success { 
            echo 'I will always say Hello Success!'
        }
        failure{ 
            echo 'I will always say Hello failure!'
        } 
    }
}
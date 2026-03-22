pipeline {
    agent {
        label 'AGENT-1'
    }
    options{
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds()
        //retry(1)
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // this will become global, we can use across pipeline
    }
    stages {
        stage('Read the version') {
            steps {
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                sh 'npm install'
                //sh 'sleep 10'
            }
        }
        stage('Docker build') {
            
            steps {
                   sh """
                        docker build -t sunilmadha996/backend:${appVersion} .
                        docker images
                   """
                }
            }
        }
        stage('Test') {
            steps {
                sh 'echo This is test'
                sh 'env'
            }
        }
        stage('Deploy') {
            when {
                //if below condition is equals to execute this stage, if not equals as below it will skip this stage
                expression { env.GIT_BRANCH != "origin/main" }  
                //branch 'production'
            }
            steps {

                    sh 'echo This is deploy'
                    //error 'pipeline failed'

            }
        }
        stage('Print Params'){
            steps{
                echo "Hello ${params.PERSON}"
                echo "Biography: ${params.BIOGRAPHY}"
                echo "Toggle: ${params.TOGGLE}"
                echo "Choice: ${params.CHOICE}"
                echo "Password: ${params.PASSWORD}"  
            }
        }

      

    post {
        always{
            echo "This sections runs always"
            deleteDir()
        }
        success{
            echo "This section run when pipeline success"
        }
        failure{
            echo "This section run when pipeline failure"
        }
    }
}
 
 
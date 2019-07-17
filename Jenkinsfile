#!groovy

pipeline {
    agent any

    stages {
        stage('Testing') {
            when { changeRequest() }
            parallel {
                stage('Code Analysis') {
                    steps {
                        sh 'sleep 1'
                    }
                }
                stage('Automatic Verification') {
                    steps {
                        sh 'sleep 1'
                    }
                }
            }
        }
    }
}

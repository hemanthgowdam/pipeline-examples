pipeline {
    agent any

    options {
        skipDefaultCheckout(true) // Skip the default checkout to control it manually
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Perform lightweight checkout from GitHub
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [[sparseCheckoutPaths: [[path: 'Jenkinsfile']]]], // Only checkout Jenkinsfile initially
                        submoduleCfg: [],
                        userRemoteConfigs: [[url: 'https://github.com/hemanthgowdam/pipeline-examples.git']]
                    ])
                }
            }
        }
        stage('Build') {
            steps {
                // Checkout the rest of the repository
                checkout scm
                // Build the project using Maven
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        // Run unit tests
                        sh 'mvn test'
                    }
                }
                stage('Performance Tests') {
                    steps {
                        // Run performance tests using JMeter
                        sh 'jmeter -n -t my_test_plan.jmx'
                    }
                }
            }
        }
        
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }

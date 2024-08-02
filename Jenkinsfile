pipeline {
    agent any
    environment {
        GITNAME = 'Subarashiman'
        GITMAIL = 'tim02366@naver.com'
        GITWEBADD = 'https://github.com/Subarashiman/fast-code.git'
        GITSSHADD = 'git@github.com:Subarashiman/fast-code.git'
        GITCREDENTIAL = 'git_cre'
        DOCKERHUB = 'subarashiman/fast'
        DOCKERHUBCREDENTIAL = 'docker_cre'
    }
    stages {
        stage('Checkout Github') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [],
                userRemoteConfigs: [[credentialsId: GITCREDENTIAL, url: GITWEBADD]]])

            }
            post {
                failure {
                    sh "echo clone failed"
                }
                success {
                    sh "echo clone success"
                }
            }
        }
        stage('start1') {
            steps {
                sh "echo hello jenkins !!!"
            }
            post {
                failure {
                    sh "echo failed"
                }
                success {
                    sh "echo success"
                }
            }
        }
        stage('start2') {
            steps {
                sh "echo hello jenkins !!!"
            }
            post {
                failure {
                    sh "echo failed"
                }
                success {
                    sh "echo success"
                }
            }
        }
        stage('start3') {
            steps {
                sh "echo hello jenkins !!!"
            }
            post {
                failure {
                    sh "echo failed"
                }
                success {
                    sh "echo success"
                }
            }
        }
        
    }
}
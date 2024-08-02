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
        stage('docker image build') {
            steps {
                sh "docker build -t ${DOCKERHUB}:${currentBuild.number} ."   // 현재 빌드 번호를 도커 이미지 태그로 갖는다.
                sh "docker build -t ${DOCKERHUB}:latest ."                   // currentBuild.number : 젠킨스가 제공하는 빌드넘버 변수

            }
            post {
                failure {
                    sh "echo image build failed"
                }
                success {
                    sh "echo image build success"
                }
            }
        }
        stage('docker image push') {
            steps {
                 withDockerRegistry(credentialsId: DOCKERHUBCREDENTIAL, url: '') {
                    sh "docker push ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker push ${DOCKERHUB}:latest"
                 }
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
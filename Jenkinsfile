pipeline {
    agent any
    environment {
        GITNAME = 'Subarashiman'
        GITMAIL = 'tim02366@naver.com'
        GITWEBADD = 'https://github.com/Subarashiman/deployment.git'
        GITSSHADD = 'git@github.com:Subarashiman/deployment.git'
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
                    sh "docker image rm -f  ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f  ${DOCKERHUB}:latest"
                    sh "echo push failed"
                }
                success {
                    sh "docker image rm -f  ${DOCKERHUB}:${currentBuild.number}"
                    sh "docker image rm -f  ${DOCKERHUB}:latest"
                    sh "echo push success"
                }
            }
        }
        stage('EKS manifest file update') {
            steps {
                git credentialsId: GITCREDENTIAL, url: GITSSHADD, branch: 'main'
                sh "git config --global user.email ${GITMAIL}"
                sh "git config --global user.email ${GITNAME}"
                sh "sed -i 's@${DOCKERHUB}:.*@${DOCKERHUB}:${currentBuild.number}@g' fast.yaml"

                sh "git add ."
                sh "git branch -M main"
                sh "git commit -m 'fixed tag ${currentBuild.number}'"
                sh "git remote remove origin"
                sh "git remote add origin ${GITSSHADD}"
                sh "git push origin main"
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
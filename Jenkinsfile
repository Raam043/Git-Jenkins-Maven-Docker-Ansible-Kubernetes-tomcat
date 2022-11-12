pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Raam043/Git-Jenkins-Maven-Docker-Ansible-Argo-Kubernetes.git'
            }
        }
        stage('Maven Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Image Build') {
            steps {
                sh 'cp /var/lib/jenkins/workspace/${JOB_NAME}/target/webapp.war /var/lib/jenkins/workspace/${JOB_NAME}/'
                sh 'docker image rm -f raam043/tomcat-project & docker build -t raam043/tomcat-project .'
            }
        }
        stage('Docker Image Push to DockerHub') {
            steps {
                withCredentials([string(credentialsId: 'DP', variable: 'DP')]) {
                    sh 'docker login -u raam043 -p ${DP}'
                    sh 'docker push raam043/tomcat-project'
            }
        }
        }
        stage('Ansible-Playbook for Deploying application on Kubernetes') {
            steps {
                ansiblePlaybook credentialsId: 'Ansible', disableHostKeyChecking: true, installation: 'Ansible', inventory: 'hosts', playbook: 'ansible_4_Kube.yml'
            }
        }
    }
}

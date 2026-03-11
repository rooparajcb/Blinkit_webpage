pipeline {

agent any

environment {
DOCKERHUB = "roopacb/Blinkit_webpage"
}

stages {

stage('Clone Repository') {
steps {
    git branch: 'main', url:'https://github.com/rooparajcb/Blinkit_webpage.git'
}
}
stage("Docker Version") {
steps {
     sh "sudo docker --version"
}
}
stage('Print Info') {
steps {
    sh """
    whoami
    hostname
    echo $JOB_NAME
    """
}
}

stage('Build Docker Image') {
steps {
sh 'sudo docker build -t $DOCKERHUB .'
}
}
    
stage('Push Docker Image') {
steps {
withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'USER', passwordVariable: 'PASS')]) {

sh '''
docker login -u $USER -p $PASS
docker push $DOCKERHUB
'''
}
}
}

stage('Deploy using Ansible') {
steps {
sh "ansible-playbook -i hosts deploy.yml --ssh-extra-args='-o StrictHostKeyChecking=no'"
}
}

}

}
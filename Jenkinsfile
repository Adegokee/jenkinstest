//scripted
// node{
//     stage('Build'){
//         echo "build completed"
//     }
//     stage('Test'){
//         echo "test completed"
//     }
//     stage("Integration"){
//         echo "integration completed"
//     }

// }

pipeline {
    agent any
    environment {
        dockerHome = tool 'myDocker'
        mavenHome = tool 'myMaven'
        PATH = "$dockerHome/bin:$mavenHome/bin:$PATH"
    }

    stages {
        stage('Checkout'){
            steps {
                sh 'mvn --version'
            }
}
    

    stage('Compile') {
        steps {
            sh 'mvn clean compile'
        }
    }

    stage('Test') {
        steps {
            sh 'mvn test'
        }
    }
    stage('Integrated Test') {
        steps {
            sh 'mvn failsafe:integration-test failsafe:verify'
        }
    }
    stage('package') {
        steps {
            sh 'mvn package -DskipTests'
        }
    }

    stage('Build Docker Image') {
        steps {
            script {
                dockerImage = docker.build('tech365/currency-devops');
            }
        }
    }

    stage('push docker image') {
        steps {
            script {
                docker.withRegistry('', 'dockerhub') {
                    dockerImage.push();
                    dockerImage.push('latest');
                }
            }
        }
    }
    }
}

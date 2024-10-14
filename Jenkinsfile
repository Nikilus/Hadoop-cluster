pipeline {
    agent any
    tools {
        maven 'Maven'   
        jdk 'JDK 8'     
    }
    stages {
        stage('Checkout') {
            steps {
                timeout(time: 30, unit: 'MINUTES') {
                    git branch: 'branch-3.3.5', url: 'https://github.com/apache/hadoop.git'
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package -Pdist,native -DskipTests -Dtar'
            }
        }
    }
}

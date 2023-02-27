pipeline {
    agent none
    stages {
        stage ('build') {
            agent {
                docker{
                    image 'maven:latest'
                    args '-v /root/boxfuse:/usr/src/boxfuse'
                }
            }
            stages {
                stage ('git') {
                    steps {
                        sh 'apt update && apt install git -y'
                        sh 'git clone https://github.com/boxfuse/boxfuse-sample-java-war-hello.git /usr/src/boxfuse'
                    }
                }
                stage ('build') {
                    steps {
                        sh 'mvn -f /usr/src/boxfuse/pom.xml package'
                    }
                }
            }
        }
        stage ('deploy') {
            agent none
            stages {
                stage ('deploy to tomcat') {
                    steps {
                        deploy adapters: [tomcat9(credentialsId: 'cbde0fd5-c24b-42e9-9f96-2383ded301f4', path: '', url: 'http://10.132.0.11:8080')], contextPath: 'web3', war: '/root/boxfuse/target/*.war'
                    }
                }
            }
        }
    }
}
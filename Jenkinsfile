pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('vcs') {
            steps {
                git branch: "dev", url: 'https://github.com/ashokmaddula/myspringpetclinic-clone.git'
            }
            
        }
        stage('Build the Code') {
            steps {
                withSonarQubeEnv('SONAR_SELF_HOSTED') {
                    sh script: 'mvn clean package sonar:sonar'
                }
            }
        }
        
    }

}
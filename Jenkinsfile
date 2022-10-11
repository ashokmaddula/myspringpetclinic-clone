pipeline {
    agent any
    parameters {
        string(name: 'MAVEN_GOAL', defaultValue: 'clean install', description: 'maven goal')

    }
    triggers {
        
        pollSCM('* * * * *')
    }
    stages {
        stage('vcs') {
            steps {
                git branch: "REL_INT_1.0", url: 'https://github.com/GitPracticeRepo/spring-petclinic.git'
            }
            
        }
                

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "JFROG_OCT22",
                    releaseRepo: 'qt-libs-release-local',
                    snapshotRepo: 'qt-libs-snapshot-local'
                )
        stage('Build the Code') {
            steps {
                withSonarQubeEnv('SONAR_SELF_HOSTED') {
                    sh script: 'mvn clean package sonar:sonar'
                }
            }
        }

        //stage("Quality Gate") {
        //    steps {
        //      timeout(time: 1, unit: 'HOURS') {
        //        waitForQualityGate abortPipeline: true
        //      }
        //    }
        //  }

          stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: 'MVN_DEFAULT', // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER"
                )
            }
        }

        
    }

}
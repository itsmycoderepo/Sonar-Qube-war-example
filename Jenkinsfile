pipeline{
    // agent {label 'sonar'}
    agent any
    stages{
       /*stage('Git Checkout Stage'){
            steps{
                git branch: 'main', url: 'https://github.com/itsmycoderepo/Sonar-Qube-war-example.git'
            }
         }*/       
       stage('Build Stage'){
            steps{
                sh 'mvn clean install'
            }
         }
    }
        post {
                success {
                    script {
                        def server = Artifactory.newServer(url: 'http://44.204.8.39:8081/artifactory/', credentialsId: 'Jfrog')
                        def rtMaven = Artifactory.newMavenBuild()
                        rtMaven.deployer server: server, releaseRepo: 'libs-release/', snapshotRepo: 'libs-snapshot/'
                        rtMaven.deployer server: server, releaseRepo: 'maven/', snapshotRepo: 'maven/'
                        rtMaven.tool = 'maven'
                        rtMaven.run(pom: 'pom.xml', goals: 'clean install')
                    }
                }
            }
    }

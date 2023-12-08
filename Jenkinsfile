pipeline {
    agent any 
    tools {
        maven 'maven'
    }
    environment {
        AWS_ACCESS_KEY_ID = credentials('s3-upload')
        AWS_SECRET_ACCESS_KEY = credentials('s3-upload')
        ARTIFACTS_NAME = "spring-petclinic-2.3.1.BUILD-SNAPSHOT.war"
    } 

    stages {
        stage ('clone') {
            steps {
                git branch: 'main', url: 'https://github.com/jitendrakumarpalei/pet-clinic-war-jitu.git'
            }
        }
        stage ('build') {
            steps {
                sh ("mvn clean install")
            }
        }
        stage ('test') {
            steps {
                sh "mvn test"
            }
        }
        stage ('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
            }
        }
        stage("Finding Artifacts") {
            steps {
                echo "My workspace is: ${env.WORKSPACE}"
                sh "cd ${env.WORKSPACE}/target"
                sh "ls -lrt ${env.WORKSPACE}/target"
            }
        }
        stage ('s3-upload') {
            steps {
                sh "chmod +x ${env.WORKSPACE}/s3-upload.sh"
                sh "./s3-upload.sh"
            }
        }

    }
}

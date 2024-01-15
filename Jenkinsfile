#!/usr/bin/env groovy

pipeline{
    agent any

    tools{
        maven 'maven'
    }
    stages{
        stage("build jar")
        {
            steps{
                script{
                       echo 'building jar file with maven tool'
                       sh 'mvn package'
                }
            }
        }

        stage("build docker image")
        {
            steps{
                script{
                    withCredentials([usernamePassword(credentialsID: 'w-docker-cred', usernameVariable: 'USERNAME', passwordVariable:'PASS')])
                    echo 'building docker image'
                    sh 'docker build -t walido2/demo-app:1.2 .'
                    sh 'echo $PASS | docker login -u $USERNAME --password-stdin'
                    sh 'docker push walido2/demo-app:1.2'
                }
            }
        }

        stage("deploy")
        {
            steps{
                script{
                    echo 'deploying the application ... '
                }
            }
        }
    }

}

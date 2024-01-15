#!/usr/bin/env groovy

pipeline{
    agent any

    tools{
        maven 'Maven'
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
                    echo 'building docker image'
                    withCredentials([usernamePassword(credentialsId: 'w-docker-cred', passwordVariable: 'PASS', usernameVariable: 'USERNAME')])
                    sh 'docker build -t walido2/demo-app:1.2 .'
                    sh "echo $PASS | docker login -u $USERNAME --password-stdin"
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

#!/usr/bin/env groovy

pipeline{
    agent any

    tools{
        maven 'Maven'
    }
    stages{
        stage("build jar")
        {
            echo 'building jar file with maven tool'
            sh 'mvn package'
        }

        stage("build docker image")
        {
            withCredentials([usernamePassword(credentialsID: 'w-docker-cred', usernameVariable: 'USERNAME', passwordVariable:'PASS')])
            echo 'building docker image'
            sh 'docker build -t walido2/demo-app:1.2 .'
            sh 'echo $PASS | docker login -u $USERNAME --password-stdin'
            sh 'docker push walido2/demo-app:1.2'
        }

        stage("deploy")
        {
            echo 'deploying the application ... '
        }
    }

}

#!/usr/bin/env groovy

pipeline{
    agent any

    tools{
        maven 'Maven'
    }
    stages{

        stage('increment version')
        {
            steps{
                script{
                    echo 'Incrementing the application patch version'
                    sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'

                    def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }

        stage("build jar")
        {
            steps{
                script{
                       echo 'building jar file with maven tool'
                       sh 'mvn clean package'
                }
            }
        }

        stage("build image")
        {
            steps{
                script{
                    echo 'building docker image'
                    withCredentials([usernamePassword(credentialsId: 'w-docker-cred', passwordVariable: 'PASS', usernameVariable: 'USERNAME')])
                    {
                        sh "docker build -t  walido2/demo-app:${IMAGE_NAME} ."
                        sh "echo $PASS | docker login -u $USERNAME --password-stdin"
                        sh "docker push walido2/demo-app:${IMAGE_NAME}"
                    }
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

        stage('commit version update')
        {
            steps{
                script{
                withCredentials([usernamePassword(credentialsId: 'github_cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {

                sh 'git config --global user.email "walidomar291@gmail.com"'
                sh 'git config --global user.name "Walidomar2"'
       
                sh "git remote set-url origin https://${USER}:${PASS}@git@github.com:Walidomar2/maven-app.git"
                sh 'git add .'
                sh 'git commit -m "ci: version bump"'
                sh 'git push origin HEAD:jenkins-jobs-test'
                 }

                }
            }
        }
    }
}

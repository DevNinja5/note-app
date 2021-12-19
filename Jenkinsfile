// Final Capstone Project

// 1. Take/Create a project in any language you like which should use a database to get and store data. 
//         - Test it on your local machine and it should be working.


//  2. Make a git repo for the project with proper branches and use the proper branching strategy.
//         Example
//                 - main
//                 - develop
//                 - test
//                 - prod
//                 - or anything that suits

// 3. Setup a Jenkins server on your local machine via ansible using ansible roles.

// 4. create a PR builder job that should trigger automatically and run test cases whenever there is a PR created on a feature branch.

// 5. We should only be able to merge the code into the develop branch only after the PR build passes (create appropriate branch rules).

// 6. Create a CI/CD pipeline(using a Jenkins file).

// 7. CI/CD pipeline that should trigger when we merged the develop branch into the main branch and it should do the following
//         - build/test
//         - generate artifacts
//         - Dockerize the application and follow proper versioning of docker image (later it will be used for automated deployment)
//                 - example: u can use the commit hash from git to tag the image
//         - push to docker hub (Create your own docker file)
//         - Deploy to your local Kubernetes (minikube), deployment should be done automatically as part of your CI/CD process.

// 8. Set Up a monitoring stack using Prometheus and grafana
//         - it should scrape the metrics from an endpoint
//         - visualize them on the grafana dashboard. 
//         - example: Capture memory and CPU resources metrics for your application and for cluster


pipeline {
    agent any

    environment{
        dockerhub_repo = "rahulsoni285/note-app"
        dockerhub_creds = 'dockerhub'
        dockerImage = ''
    }

    options{
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
    }
    tools{
        nodejs 'node'
        jdk 'jdk11'
    }

    stages{
        stage("Installing node_modules, packing and deployment"){
            when{
                branch 'main'
            }
            stages{
                stage("building docker image"){
                    steps{
                        script{
                            dockerImage = docker.build dockerhub_repo + ":$GIT_COMMIT-build-$BUILD_NUMBER"
                        }
                    }
                }
                 stage("Pushing the docker image"){
                    steps{
                        script {
                            docker.withRegistry('', dockerhub_creds){
                                dockerImage.push()
                                dockerImage.push('latest')
                                dockerImage.push('v1')
                            }
                        }
                    }
                }
                //  stage(""){
                //     steps{

                //     }
                // }
                
            }
        }
    }
}
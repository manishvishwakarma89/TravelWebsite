@Library('Shared') _
pipeline{
    agent { label 'MyAgent'}
    
    parameters {
        string(
            name: 'BACKEND_DOCKER_TAG',
            defaultValue: 'latest',
            description: 'Backend Docker Image Tag'
        )
        string(
            name: 'FRONTEND_DOCKER_TAG',
            defaultValue: 'latest',
            description: 'Frontend Docker Image Tag'
        )
    }
    
    stages{
        stage("Code Clone"){
            steps{
                script{
                    sh "whoami"
                    clone("https://github.com/manishvishwakarma89/TravelWebsite.git","main")
                }
            }
        }
    
        stage("Code Build"){
            steps{
                script{
                    dir('backend'){
                        docker_build(
                            "wanderlust-backend-beta",
                            params.BACKEND_DOCKER_TAG,
                            "manishvishwa801"
                        )
                    }
                    dir('frontend'){
                        docker_build(
                            "wanderlust-frontend-beta",
                            params.FRONTEND_DOCKER_TAG,
                            "manishvishwa801"
                        )
                    }
                }
            }
        }

        stage("Push to DockerHub"){
            steps{
                script{
                    docker_push(
                        "wanderlust-backend-beta",
                        "${params.BACKEND_DOCKER_TAG}",
                        "manishvishwa801"
                    )
                    docker_push(
                        "wanderlust-frontend-beta",
                        "${params.FRONTEND_DOCKER_TAG}",
                        "manishvishwa801"
                    )
                }
            }
        }

        stage("Deploy"){
            steps{
                script{
                    deploy()
                }
            }
        }
    }

    post{
        success{
            echo "Pipeline succeeded"
        }
        failure{
            echo "Pipeline failed"
        }
    }
}

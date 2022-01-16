pipeline {    
  parameters {

    choice(name: 'deploy_env_choice',
      choices: 'default\ndev\nuat',
      description: 'deploy_all_fra_services')  
    string(name: 'deploy_reason',
      defaultValue: 'General upgrading or Tesing new integration feature',
      description: 'Given some reson when deploy all')
  }

    agent none

    stages {

        stage('Clone repository') {

            agent any

            steps {
            /* Clone our repository */
                checkout scm
            }    
        }

        stage('Deploy requirement') {

            agent any
            
            steps {
                echo 'Will do follow list'
                echo "Chose Env: ${params.deploy_env_choice}"
                echo "Deploy all services"
                echo "Why Deploy all: ${deploy_reason}"
            }
        }

        stage('Deploying all services') {

            agent any
            
            steps {
                input message: "Prepare parallel to Process deploying all services"
                echo "deploying all ..."
            }
        }


        stage('Preparing image for all services') {
            parallel {
                stage("pulling employee service image") {
                    
                    agent any 

                    steps {
                        
                        echo "pull employee-servic image"
                        dir("employee-service") {
                            
                            echo "Git deploy yaml in project employee-service"
                            git branch: 'main', credentialsId: '8c993f8f-5bbe-4ba8-b4a7-f673ff0d6f26', url: 'https://github.com/dekiyy/employee-service.git'

                            withEnv([
                                        "SERVICE=employee",
                                        "TAG=1.1"
                                    ]){
                                    /* Pull the docker image */
                                        echo "Downloading the image  ..."
                                        // sh "docker pull ${SERVICE}:${TAG}"
                                        echo "Is the image canbe run ?"
                                        sh "docker image inspect ${SERVICE}:${TAG}"
                                    }
                        }
                    }
                }

                stage("pulling department service image") {
                    
                    agent any 

                    steps {
                        
                        echo "pull department-servic image"
                        dir("department-service") {
                            
                            echo "Git deploy yaml in project department-service"
                            git branch: 'main', credentialsId: '8c993f8f-5bbe-4ba8-b4a7-f673ff0d6f26', url: 'https://github.com/dekiyy/department-service.git'

                            withEnv([
                                        "SERVICE=department",
                                        "TAG=1.1"
                                    ]){
                                    /* Pull the docker image */
                                        echo "Downloading the image  ..."
                                        // sh "docker pull ${SERVICE}:${TAG}"
                                        echo "Is the image canbe run ?"
                                        sh "docker image inspect ${SERVICE}:${TAG}"
                                    }
                        }
                    }
                }
                
                stage("pulling organization service image") {
                    
                    agent any 

                    steps {
                        
                        echo "pull organization-servic image"
                        dir("organization-service") {
                            
                            echo "Git deploy yaml in project organization-service"
                            git branch: 'main', credentialsId: '8c993f8f-5bbe-4ba8-b4a7-f673ff0d6f26', url: 'https://github.com/dekiyy/organization-service.git'

                            withEnv([
                                        "SERVICE=organization",
                                        "TAG=1.1"
                                    ]){
                                    /* Pull the docker image */
                                        echo "Downloading the image  ..."
                                        // sh "docker pull ${SERVICE}:${TAG}"
                                        echo "Is the image canbe run ?"
                                        sh "docker image inspect ${SERVICE}:${TAG}"
                                    }
                        }
                    }
                }
            }
        }

        stage('Deploying employee services') {
            parallel {
                stage("Deploying employee service") {
                    
                    agent any 
    
                    steps {
                        
                        echo "deploy employee-service"
                        dir("employee-service") {
                            
                            echo "in project employee-service"
                            echo "ls"

                            withEnv([
                                        "SERVICE=employee",
                                        "NAMESPACE=${params.deploy_env_choice}",
                                        "TAG=1.1"
                                    ]){
                                        echo "Deploy image to k8s cluster [env ${params.deploy_env_choice}]  ..."
                                        sh "./deployment/deploy.sh"
                                    }
                        }
                    }
                }
                stage("Deploying department service") {
                    
                    agent any 
 
                    steps {
                        
                        echo "deploy department-service"
                        dir("department-service") {
                            
                            echo "in project department-service"
                            echo "ls"

                            withEnv([
                                        "SERVICE=department",
                                        "NAMESPACE=${params.deploy_env_choice}",
                                        "TAG=1.1"
                                    ]){
                                        echo "Deploy image to k8s cluster [env ${params.deploy_env_choice}]  ..."
                                        sh "./deployment/deploy.sh"
                                    }
                        }
                    }
                }
                stage("Deploying organization service") {
                    
                    agent any 
 
                    steps {
                        
                        echo "deploy organization"
                        dir("organization-service") {
                            
                            echo "in project organization"
                            echo "ls"

                            withEnv([
                                        "SERVICE=organization",
                                        "NAMESPACE=${params.deploy_env_choice}",
                                        "TAG=1.1"
                                    ]){
                                    /* Build the docker image */
                                        echo "Deploy image to k8s cluster [env ${params.deploy_env_choice}]  ..."
                                        sh "./deployment/deploy.sh"
                                    }
                        }
                    }
                }
            }
        }
   
    }
}
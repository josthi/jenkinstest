//@Library('pipeline-logparser') _
pipeline {
    agent any
    options {
       timestamps ()
       buildDiscarder(logRotator(numToKeepStr: '30'))
    }
    parameters {
        choice(name: 'domain_choice',
          choices: 'buss\ncorp',
          description: 'Select domain to deploy?')     
        choice(name: 'environment_choice',
          choices: 'test\npreprod\nprod',
          description: 'Select environment to deploy?')
        choice(name: 'component_choice',
          choices: 'gke\nmaptiler\nmonitor\nnexus\njenkins\nproject',
          description: 'Select component to deploy?')
        choice(name: 'command_choice',
          choices: 'plan\napply\nsuperapply',
          description: 'Select command to deploy?')
    }
    stages {
        stage('ValidateParams') {
            steps {
                script{
                    if(params.component_choice.equals("jenkins") && params.domain_choice.equals("corp")) {
                        echo "Jenkins shouldn't be deployed on the Corp Domain. Aborting the deployment"
                        currentBuild.result = 'ABORTED'
                        return
                    }
                }
            }
        }
        //stage('SCM Checkout') {
            //steps {
                //checkout([
                    //$class: 'GitSCM',
                    //branches: [[name: '*/master']],
                    //extensions: [[
                        //$class: 'SubmoduleOption',
                        //isableSubmodules: false,
                        //parentCredentials: true,
                        //recursiveSubmodules: true,
                        //reference: '',
                        //trackingSubmodules: true
                    //]],
                    //serRemoteConfigs: [[
                        //credentialsId: 'clone',
                        //url: 'git@git@github.com:josthi/jenkinstest.git'
                    //]
                //)     
            //}
        //}

        stage('Deploy') {
            steps {
                echo "Environment ${params.environment_choice}"
                echo "Domain ${params.domain_choice}"
                echo "Component ${params.component_choice}"
                echo "Terraform Command ${params.command_choice}"
                echo "Starting the deployment"
                sh "build_cluster.sh ${params.domain_choice} ${params.environment_choice} ${params.component_choice} ${params.command_choice}"
            }
        }
    }

}

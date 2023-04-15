pipeline {
    agent any
    parameters { 
        string(name: 'COMMIT', defaultValue: '', description: 'Enter the commit hash or branch name that you would like to build from.')
        string(name: 'IMAGE_NAME', defaultValue: '', description: 'Name of image being built taken from Images directory name')
        choice(name: 'ENVIRONMENT', choices: ['dev', 'prod'], description: 'Choose the environment where this pipline is running.')
    }
     environment {
        CODEBUILD_PROJECT = "Build-Pipeline-Images_${params.ENVIRONMENT}"
    }
    
    stages {
        stage('Setup') {
            steps{
            echo "DEBUG: parameter COMMIT = ${params.COMMIT}"
            echo "DEBUG: parameter IMAGE_NAME = ${params.IMAGE_NAME}"
            echo "DEBUG: parameter ENVIRONMENT = ${params.ENVIRONMENT}"
            echo "DEBUG: env value CODEBUILD_PROJECT = ${CODEBUILD_PROJECT}"

            script {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: params.COMMIT]],
                    userRemoteConfigs: scm.userRemoteConfigs
                    ])
                    echo "Checked out commit ${params.COMMIT}"
                } 
            }// end stage Setup
        }

        stage('BuildPushImage') {
            environment { 
                BUILDSPEC_FILE = 'buildspec.yml' 
            }
            steps {
                echo "### starting stage BuildPushImage ###"
                echo "BUILDSPEC_FILE: ${BUILDSPEC_FILE}"
                script{
                    awsCodeBuild    projectName: "${CODEBUILD_PROJECT}",
                                    buildSpecFile: "${BUILDSPEC_FILE}",
                                    envVariables: "[ { COMMIT, $params.COMMIT }, { IMAGE_NAME, $params.IMAGE_NAME } ]",
                                    credentialsType: 'keys',
                                    region: 'us-east-1',
                                    sourceControlType: 'jenkins'
                }
            }
        } // end stage BuildPushImage
    } // end Stages
}
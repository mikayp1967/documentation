# Jenkins pipelines:


## General syntax

    pipeline {
        agent { node { label 'agent-type' } }
        stages {
            stage('Stage 1') {
                steps {
                    
                }
            }
            stage('Stage 2') {
                steps {
                    
                }
            }
        }
    }



## Credential blocks

    withCredentials([
        usernamePassword(credentialsId: 'credential_name', 
        passwordVariable: 'PASSWORD', 
        usernameVariable: 'USERNAME'
    )]) {
        sh '** code block here **'
    }


## AWS Creds

    withCredentials([[
        $class: 'AmazonWebServicesCredentialsBinding',
        credentialsId: 'aws_keys',
        accessKeyVariable: 'CREDENTIAL_AWS_ACCESS_KEY_ID',
        secretKeyVariable: 'CREDENTIAL_AWS_SECRET_ACCESS_KEY'
    ]]) {
        sh '** code block here **'
    }

## Conditional input/confirmation for given condition

    when {
        beforeInput true
        branch 'master'
    }
    input {
        message "Apply ?"
        ok "Confirmation message"
        parameters {
            string(name: 'Confirm deployent: ', defaultValue: 'Approver: ', description: 'Please confirm you wish to delpoy')
        }
    }
    steps {
        ....
    }


## Set parameters within Jenkinsfile

    BUILD_TYPE - CHOICE PARAMETER
    - Allowed Values:
        FRONT_END
        BACK_END
        BOTH



## Stashing files:

    stash name: 'initialPull', includes: '**/outputDir/class-direct/build_process/**'



## Loading and Using functions

    def myFunction = load './myCode.groovy'
    myCode.functionName(
        'param1',
        'param2',
        "param3",
        [
        'List val 1': 'List val 1b',
        'List val 2': 'List val 2b',
        ]
    )

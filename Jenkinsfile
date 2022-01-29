import groovy.json.JsonSlurperClassic
def jsonParse(def json) {
    new groovy.json.JsonSlurperClassic().parseText(json)
}
pipeline {
    agent any
    environment {
        NEXUS_USER         = credentials('token-nexus-curl-useradmin')
        NEXUS_PASSWORD     = credentials('token-nexus-curl-passadmin')
    }
    parameters {
        choice(
            name:'compileTool',
            choices: ['Maven', 'Gradle'],
            description: 'Seleccione herramienta de compilacion'
        )
    }
    stages {
        stage("Pipeline"){
            steps {
                script{
                env.STAGE  = env.STAGE_NAME
                print 'Compile Tool: ' + params.compileTool;
                switch(params.compileTool)
                    {
                        case 'Maven':
                            figlet  "Maven"
                            def ejecucion = load 'maven.groovy'
                            ejecucion.call()
                        break;
                        case 'Gradle':
                            figlet  "Gradle"
                            def ejecucion = load 'script.groovy'
                            ejecucion.call()
                        break;
                    }
                }
            }
        }
    }
    post {
        success{
            slackSend color: 'good', message: "[Felipe Contreras][${JOB_NAME}][${params.compileTool}] Ejecución Exitosa.", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'token-slack'
        }
        failure{
            slackSend color: 'danger', message: "[Felipe Contreras][${JOB_NAME}][${params.compileTool}] Ejecucion fallida en stage [${env.STAGE}]", teamDomain: 'dipdevopsusac-tr94431', tokenCredentialId: 'token-slack'
        }
    }
}
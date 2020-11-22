pipeline {
    agent any
    tools {
        maven 'Maven3'
        jdk 'Java9'
    }
    options {
        buildDiscarder logRotator(numToKeepStr: '100')
    }
    stages {
        stage('Generate') {
            steps {
                sh "mvn -Dresource.name=${name} -Dresource.author=${author} -Dresource.id=${resourceId} -Dresource.website=${website} -Dresource.description=\"${description}\" clean package"
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/loader.jar'
            }
        }
        stage('Callback') {
            steps {
                script {
                    withCredentials([string(credentialsId: JAVADOCS_TOKEN_CREDENTIAL_ID, variable: 'SECRET')]) {
                        httpRequest(acceptType: 'APPLICATION_JSON', contentType: 'APPLICATION_OCTETSTREAM',
                             httpMode: 'POST', ignoreSslErrors: true, timeout: 3000,
                             multipartName: 'file',
                             responseHandle: 'NONE',
                             uploadFile: "target/loader.jar",
                             customHeaders:[[name:'secret', value:"${SECRET}", maskValue:true]],
                             url: "192.168.1.14/ResourceLoaderBuildCallback?resourceId=${resourceId}")
                    }
                }
            }
        }
    }
}

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
                sh "mvn -Dresource.name=${name} -Dresource.author=${author} -Dresource.id=${resourceId} -Dresource.website=${website} -Dresource.description=${description} clean package"
            }
        }
        stage('Archive') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar'
            }
        }
    }
}
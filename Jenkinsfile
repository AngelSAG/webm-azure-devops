pipeline {
    agent { label 'docker1' }
    parameters {
	    string(name: 'username', defaultValue: '', description: 'Username') 
		string(name: 'password', defaultValue: '', description: 'password') 
        string(name: 'targetDockerRegistryHost', defaultValue: 'daerepository03.eur.ad.sag:4443', description: 'Target registry host') 
        string(name: 'targetDockerRegistryOrg', defaultValue: 'sagdevops', description: 'Target registry organization') 
        string(name: 'targetDockerRepoName', defaultValue: 'webmethods-microservicesruntime', description: 'Target docker repo name') 
        string(name: 'targetDockerRepoTag', defaultValue: '10.5.0.0', description: 'Target docker repo tag') 

        string(name: 'sourceDockerRegistryHost', defaultValue: 'docker.io', description: 'Source registry host') 
        string(name: 'sourceDockerRegistryOrg', defaultValue: 'store/softwareag', description: 'SOurce registry organization') 
        string(name: 'sourceDockerRepoName', defaultValue: 'webmethods-microservicesruntime', description: 'Source docker repo name') 
        string(name: 'sourceDockerRepoTag', defaultValue: '10.5.0.0', description: 'Source docker repo tag') 
    }
    environment {
      REG_HOST="${params.sourceDockerRegistryHost}"
      REG_ORG="${params.sourceDockerRegistryOrg}"
      REPO_NAME="${params.sourceDockerRepoName}"
      REPO_TAG="${params.sourceDockerRepoTag}"
      TARGET_REG_HOST="${params.targetDockerRegistryHost}"
      TARGET_REG_ORG="${params.targetDockerRegistryOrg}"
      TARGET_REPO_NAME="${params.targetDockerRepoName}"
      TARGET_REPO_TAG="${params.targetDockerRepoTag}"
    }
    stages {
        stage('docker login') {
            steps{
                script {
                  sh "echo ${params.password} | docker login -u ${params.username} --password-stdin $REG_HOST"
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh "docker-compose -f containers/docker-compose.yml config"
                    sh "docker-compose -f containers/docker-compose.yml build"
                    }
                }
            }
        stage('Run') {
            steps {
                script {
                     sh "docker-compose -f containers/docker-compose.yml run microservices-runtime"
                }
            }
        }
    }
}
// def jenkinsFile
// stage('Loading Jenkins file') {
//     jenkinsFile = fileLoader.fromGit('project-Jenkins/project', 'https://github.com/GajananHegde/Jenkins-repo', 'main', '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', '')
// }


pipeline {
  agent any
  parameters {
        text(name: 'BIOGRAPHY', defaultValue: 'Dheera_Dheera', description: 'Enter some information about the person')
        choice(name: 'UPSTREAM_DATABASE', choices: ['wells', 'hnrblock','Two', 'Three','one'], description: 'Pick something')
        choice(name: 'DOWNSTREAM_DATABASE', choices: ['markey', 'hnr-qa', 'Two', 'Three','two'], description: 'Pick something')
    }

  stages {
    stage('Loading Project code repo'){
      steps {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
          doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
          submoduleCfg: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/GajananHegde/project-Jenkins']]]
      }
    }
    stage('Loading Jenkins file'){
      steps {
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
          doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
          submoduleCfg: [], userRemoteConfigs: [[credentialsId: '003a4450-bafd-4f3d-be4e-75d1accfcdf8', url: 'https://github.com/GajananHegde/Jenkins-repo']]]
      }
    }
    stage('Execute stuff'){
      parallel {
        stage('Task1')
        {
          steps{
            script {
              mainfunc("${params.UPSTREAM_DATABASE}", "${params.DOWNSTREAM_DATABASE}")
            }
          }
        }
      }
    }
  }
}

def inject_env(deploy_environment){
    switch (deploy_environment){
        case 'wells':
            env.build_env = deploy_environment
            env.environment_starting_letter = 'p'
            env.subnets = 'subnet-009b9198c8c676ea5'
            env.security_groups = 'sg-0cf66b11b856e9af5'
            env.environment = 'prod'
            break
        case 'hnrblock':
            env.build_env = deploy_environment
            env.environment_starting_letter = 'p'
            env.subnets = 'subnet-009b9198c8c676ea5'
            env.security_groups = 'sg-0cf66b11b856e9af5'
            env.environment = 'prod'
            break
        case 'cft-qa':
            env.build_env = deploy_environment
            env.environment_starting_letter = 'q'
            env.subnets = '\'subnet-0a1807c169d4ba548\',\'subnet-06163ecaf0273e89d\',\'subnet-0922504ca7a88b1f7\''
            env.security_groups = 'sg-0d68a6694858e166a'
            env.environment = 'dev'
            break
        case 'markey':
            env.build_env = deploy_environment
            env.environment_starting_letter = 'q'
            env.subnets = '\'subnet-0a1807c169d4ba548\',\'subnet-06163ecaf0273e89d\',\'subnet-0922504ca7a88b1f7\''
            env.security_groups = 'sg-0d68a6694858e166a'
            env.environment = 'dev'
            break
        case 'hnr-qa':
            env.build_env = deploy_environment
            env.environment_starting_letter = 'q'
            env.subnets = '\'subnet-0a1807c169d4ba548\',\'subnet-06163ecaf0273e89d\',\'subnet-0922504ca7a88b1f7\''
            env.security_groups = 'sg-0d68a6694858e166a'
            env.environment = 'dev'
    }
}

// def mainfunc(String build_branch, String build_number, String build_job, String build_url) {
def mainfunc(String from_db, String to_db){
    inject_env(from_db)
    sh """
    echo "build_env :${build_env}"
    """
    db_sync_from()
    env.upstream_environment = "${env.environment}"
    env.upstream_build_env = "${env.build_env}"
    inject_env(to_db)
    sh """
    sleep 20
    echo --network-configuration "awsvpcConfiguration={subnets=["${subnets}"],securityGroups=["${security_groups}"],assignPublicIp='ENABLED'}"
    """
    db_sync_to()
}

def db_sync_from()
{
    sh """
    /opt/homebrew/bin/aws ecs run-task \
       --cluster p-dash-rdsbackup \
       --task-definition rdsbackup-taskdefinitions:3 \
       --launch-type FARGATE \
       --overrides '{"containerOverrides": [{"name": "rdsbackup", "environment": [{ "name": "IS_DOWNSYNC_DB", "value": "True"},{"name": "ISFROM", "value": "True"},{"name": "UPSTREAM_CLIENT_NAME_BUILD", "value": "${build_env}"},{"name": "UPSTREAM_ENV_LETTER_BUILD", "value": "${environment_starting_letter}"},{"name": "TARGETPATH", "value": "${environment}/${build_env}/${build_env}-${env.BUILD_NUMBER}"}]}]}' \
       --network-configuration "awsvpcConfiguration={subnets=["${subnets}"],securityGroups=["${security_groups}"],assignPublicIp='ENABLED'}"
    """
}

def db_sync_to()
{
    sh """
    /opt/homebrew/bin/aws ecs run-task \
        --cluster p-dash-rdsbackup \
        --task-definition rdsbackup-taskdefinitions:3 \
        --launch-type FARGATE \
        --overrides '{"containerOverrides": [{"name": "rdsbackup", "environment": [{ "name": "IS_DOWNSYNC_DB", "value": "True"},{"name": "ISFROM", "value": "False"},{"name": "DOWNSTREAM_CLIENT_NAME_BUILD", "value": "${build_env}"},{"name":"DOWNSTREAM_ENV_LETTER_BUILD", "value": "${environment_starting_letter}"},{"name": "SOURCEPATH", "value": "${upstream_environment}/${upstream_build_env}/${upstream_build_env}-${env.BUILD_NUMBER}"},{"name": "TARGETPATH", "value": "${environment}/${build_env}/${build_env}-${env.BUILD_NUMBER}"}]}]}' \
        --network-configuration "awsvpcConfiguration={subnets=["${subnets}"],securityGroups=["${security_groups}"],assignPublicIp='ENABLED'}"
    """
}
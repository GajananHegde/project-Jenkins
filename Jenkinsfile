def jenkinsFile
stage('Loading Jenkins file') {
    jenkinsFile = fileLoader.fromGit('project-Jenkins/project', 'https://github.com/GajananHegde/Jenkins-repo', 'main', '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', '')
}


pipeline {
  agent any
  parameters {
        text(name: 'BIOGRAPHY', defaultValue: 'Dheera_Dheera', description: 'Enter some information about the person')
        choice(name: 'UPSTREAM_DATABASE', choices: ['wells', 'Two', 'Three','one'], description: 'Pick something')
        choice(name: 'DOWNSTREAM_DATABASE', choices: ['qa', 'cft-qa', 'Two', 'Three','two'], description: 'Pick something')
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
              jenkinsFile.mainfunc("${params.UPSTREAM_DATABASE}", "${params.DOWNSTREAM_DATABASE}")
            }
          }
        }
      }
    }
  }
}

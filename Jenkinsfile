def jenkinsFile
stage('Loading Jenkins file') {
    jenkinsFile = fileLoader.fromGit('Test-2/Jenkinsfile', 'https://github.com/robinaa/jenkinsfilerepo', 'develop', 'bccfcd00-38b2-4b1f-a943-0cc936c415e2', '')
}


pipeline {
  agent any

  stages {
    stage('Loading Jenkins file'){
      environment {
        build_branch = "${env.BRANCH_NAME}"
        build_number = "${env.BUILD_NUMBER}"
        build_job = "${env.JOB_NAME}"
        build_url = "${env.BUILD_URL}"
      }
      steps {
        // Checkout code from Jenkinsfile-repo
        // git(url: '', branch: '')
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/develop']], 
          doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
          submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'bccfcd00-38b2-4b1f-a943-0cc936c415e2', url: 'https://github.com/robinaa/jenkinsfilerepo']]]
        // sh "echo Pipeline Build Number: ${build_number}"
        // sh "echo Pipeline Build Job: ${build_job}"
        // sh "echo Pipeline Build URL: ${build_url}"
        // sh "ls -lathr"
        // sh "ls -lathr Build-Dir/"
      }
    }
    stage('Execute stuff'){
      environment {
        build_branch = "${env.BRANCH_NAME}"
        build_number = "${env.BUILD_NUMBER}"
        build_job = "${env.JOB_NAME}"
        build_url = "${env.BUILD_URL}"
        parallel_stage_1 = 'Frontend'
        parallel_stage_2 = 'Backend'

      }      
      parallel {
        stage('Task1')
        {
          steps{
            script {
              // jenkinsFile.mainfunc(build_branch, build_job, build_number, build_url)
              jenkinsFile.mainfunc(parallel_stage_1)
            }
          }
        }
        stage('Task2')
        {
          steps {
            script {
              // jenkinsFile.mainfunc2(build_branch, build_job, build_number, build_url)
              jenkinsFile.mainfunc(parallel_stage_2)
            }
          }
        }
      }
    }
  }
}

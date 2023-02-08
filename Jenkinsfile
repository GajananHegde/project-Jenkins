def jenkinsFile
stage('Loading Jenkins file') {
    jenkinsFile = fileLoader.fromGit('project-Jenkins/project', 'https://github.com/GajananHegde/Jenkins-repo', 'main', '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', '')
}


pipeline {
  agent any
  parameters {
        text(name: 'BIOGRAPHY', defaultValue: 'Dheera_Dheera', description: 'Enter some information about the person')
        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        extendedChoice(
        name: 'tagName',
        defaultValue: '',
        description: 'tag name',
        type: 'PT_SINGLE_SELECT',
        groovyScript: """def gettags = ("git ls-remote -t https://github.com/GajananHegde/project-Jenkins.git").execute()
          return gettags.text.readLines().collect { it.split()[1].replaceAll('refs/tags/', '').replaceAll("\\\\^\\\\{\\\\}", '')}
              """,)
        extendedChoice(
        name: 'choicesCheckbox',
        defaultValue: 'rick',
        multiSelectDelimiter: ',',
        type: 'PT_CHECKBOX',
        value: 'rick,morty,jerry,summer,beth,birbperson'
        )
    }

  stages {
    stage('Loading Project code repo \"${params.BIOGRAPHY}\"'){
      environment {
        build_branch = "${env.BRANCH_NAME}"
        build_number = "${env.BUILD_NUMBER}"
        build_job = "${env.JOB_NAME}"
        build_url = "${env.BUILD_URL}"
      }
      steps {
        // Checkout code from Jenkinsfile-repo
        // git(url: '', branch: '')
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
          doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
          submoduleCfg: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/GajananHegde/project-Jenkins']]]
        // sh "echo Pipeline Build Number: ${build_number}"
        // sh "echo Pipeline Build Job: ${build_job}"
        // sh "echo Pipeline Build URL: ${build_url}"
        // sh "ls -lathr"
        // sh "ls -lathr Build-Dir/"
      }
    }
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
        checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
          doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
          submoduleCfg: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/GajananHegde/Jenkins-repo']]]
        // sh "Checking out Jenkins repo"
        // sh "echo Pipeline Build Job: ${build_job}"
        // sh "echo Pipeline Build URL: ${build_url}"
        // sh "ls -lathr"
        // sh "ls -lathr Build-Dir/"
      }
    }
    stage('Execute stuff'){
      environment {
        build_branch = "${env.BRANCH_NAME}"
    //     build_number = "${env.BUILD_NUMBER}"
    //     build_job = "${env.JOB_NAME}"
    //     build_url = "${env.BUILD_URL}"
        parallel_stage_1 = 'Frontend'
    //     parallel_stage_2 = 'Backend'

      }
      parallel {
        stage('Task1')
        {
          steps{
            // some_function()
            // echo "${env.BIOGRAPHY}"+"${params.CHOICE}"
            // echo "${params.tagName}"
            // sh """
            // ls -l .Build-Dir
            // cd .Build-Dir
            // pwd
            // cd project-Jenkins
            // ls -al
            // """
            script {
            //   echo "${env.BIOGRAPHY}"
            //   currentBuild.description = "env: ${params.BIOGRAPHY} tagName: ${params.CHOICE}"
            //   // jenkinsFile.mainfunc(build_branch, build_job, build_number, build_url)
              jenkinsFile.mainfunc("${params.tagName}", "wells")
            }
          }
        }
        // stage('Task2')
        // {
        //   steps {
        //     echo '${env.BIOGRAPHY}'
            // script {
              // jenkinsFile.mainfunc2(build_branch, build_job, build_number, build_url)
              // jenkinsFile.mainfunc(parallel_stage_2, ${params.BIOGRAPHY})
            // }
        //   }
        // }
      }
    }
  }
}

def some_function() {
  print(params.BIOGRAPHY)
}

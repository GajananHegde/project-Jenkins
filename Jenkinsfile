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
    }

  stages {
    stage("Release scope") {
            steps {
                script {
                    // This list is going to come from a file, and is going to be big.
                    // for example purpose, I am creating a file with 3 items in it.
                    sh "echo \"first\nsecond\nthird\" > ${WORKSPACE}/list"

                    // Load the list into a variable
                    env.LIST = readFile("${WORKSPACE}/list").replaceAll(~/\n/, ",")

                    env.RELEASE_SCOPE = input message: 'User input required', ok: 'Release!',
                            parameters: [extendedChoice(
                            name: 'ArchitecturesCh',
                            defaultValue: "${env.BUILD_ARCHS}",
                            multiSelectDelimiter: ',',
                            type: 'PT_CHECKBOX',
                            value: env.LIST
                      )]
                      // Show the select input
                      env.RELEASE_SCOPE = input message: 'User input required', ok: 'Release!',
                            parameters: [choice(name: 'CHOOSE_RELEASE', choices: env.LIST, description: 'What are the choices?')]
                }
                echo "Release scope selected: ${env.RELEASE_SCOPE}"
            }
        }
    stage('Loading Project code repo'){
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
              jenkinsFile.mainfunc("${params.choicesCheckbox}", "Frontend")
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

// def jenkinsFile
// stage('Loading Jenkins file') {
//     jenkinsFile = fileLoader.fromGit('project-Jenkins/project', 'https://github.com/GajananHegde/Jenkins-repo', 'main', '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', '')
// }


// pipeline {
//   agent any
//   parameters {
//         text(name: 'BIOGRAPHY', defaultValue: 'Dheera_Dheera', description: 'Enter some information about the person')
//         choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
//         extendedChoice(
//         name: 'tagName',
//         defaultValue: '',
//         description: 'tag name',
//         type: 'PT_SINGLE_SELECT',
//         groovyScript: """def gettags = ("git ls-remote -t https://github.com/GajananHegde/project-Jenkins.git").execute()
//           return gettags.text.readLines().collect { it.split()[1].replaceAll('refs/tags/', '').replaceAll("\\\\^\\\\{\\\\}", '')}
//               """,)
//     }

//   stages {
//     stage("Release scope") {
//             steps {
//                 script {
//                     // This list is going to come from a file, and is going to be big.
//                     // for example purpose, I am creating a file with 3 items in it.
//                     sh "echo \"first\nsecond\nthird\" > ${WORKSPACE}/list"

//                     // Load the list into a variable
//                     env.LIST = readFile("${WORKSPACE}/list").replaceAll(~/\n/, ",")

//                     env.RELEASE_SCOPE = input message: 'User input required', ok: 'Release!',
//                             parameters: [extendedChoice(
//                             name: 'ArchitecturesCh',
//                             defaultValue: "${env.BUILD_ARCHS}",
//                             multiSelectDelimiter: ',',
//                             type: 'PT_CHECKBOX',
//                             value: env.LIST
//                       )]
//                       // Show the select input
//                       env.RELEASE_SCOPE = input message: 'User input required', ok: 'Release!',
//                             parameters: [choice(name: 'CHOOSE_RELEASE', choices: env.LIST, description: 'What are the choices?')]
//                 }
//                 echo "Release scope selected: ${env.RELEASE_SCOPE}"
//             }
//         }
//     stage('Loading Project code repo'){
//       environment {
//         build_branch = "${env.BRANCH_NAME}"
//         build_number = "${env.BUILD_NUMBER}"
//         build_job = "${env.JOB_NAME}"
//         build_url = "${env.BUILD_URL}"
//       }
//       steps {
//         // Checkout code from Jenkinsfile-repo
//         // git(url: '', branch: '')
//         checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
//           doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
//           submoduleCfg: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/GajananHegde/project-Jenkins']]]
//         // sh "echo Pipeline Build Number: ${build_number}"
//         // sh "echo Pipeline Build Job: ${build_job}"
//         // sh "echo Pipeline Build URL: ${build_url}"
//         // sh "ls -lathr"
//         // sh "ls -lathr Build-Dir/"
//       }
//     }
//     stage('Loading Jenkins file'){
//       environment {
//         build_branch = "${env.BRANCH_NAME}"
//         build_number = "${env.BUILD_NUMBER}"
//         build_job = "${env.JOB_NAME}"
//         build_url = "${env.BUILD_URL}"
//       }
//       steps {
//         // Checkout code from Jenkinsfile-repo
//         // git(url: '', branch: '')
//         checkout changelog: false, poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/main']],
//           doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: '.Build-Dir']],
//           submoduleCfg: [], userRemoteConfigs: [[credentialsId: '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', url: 'https://github.com/GajananHegde/Jenkins-repo']]]
//         // sh "Checking out Jenkins repo"
//         // sh "echo Pipeline Build Job: ${build_job}"
//         // sh "echo Pipeline Build URL: ${build_url}"
//         // sh "ls -lathr"
//         // sh "ls -lathr Build-Dir/"
//       }
//     }
//     stage('Execute stuff'){
//       environment {
//         build_branch = "${env.BRANCH_NAME}"
//     //     build_number = "${env.BUILD_NUMBER}"
//     //     build_job = "${env.JOB_NAME}"
//     //     build_url = "${env.BUILD_URL}"
//         parallel_stage_1 = 'Frontend'
//     //     parallel_stage_2 = 'Backend'

//       }
//       parallel {
//         stage('Task1')
//         {
//           steps{
//             // some_function()
//             // echo "${env.BIOGRAPHY}"+"${params.CHOICE}"
//             // echo "${params.tagName}"
//             // sh """
//             // ls -l .Build-Dir
//             // cd .Build-Dir
//             // pwd
//             // cd project-Jenkins
//             // ls -al
//             // """
//             script {
//             //   echo "${env.BIOGRAPHY}"
//             //   currentBuild.description = "env: ${params.BIOGRAPHY} tagName: ${params.CHOICE}"
//             //   // jenkinsFile.mainfunc(build_branch, build_job, build_number, build_url)
//               jenkinsFile.mainfunc("${params.choicesCheckbox}", "Frontend")
//             }
//           }
//         }
//         // stage('Task2')
//         // {
//         //   steps {
//         //     echo '${env.BIOGRAPHY}'
//             // script {
//               // jenkinsFile.mainfunc2(build_branch, build_job, build_number, build_url)
//               // jenkinsFile.mainfunc(parallel_stage_2, ${params.BIOGRAPHY})
//             // }
//         //   }
//         // }
//       }
//     }
//   }
// }

// def some_function() {
//   print(params.BIOGRAPHY)
// }

// #!groovy
// @Library('gale43-library') _

/**
 * The _ here is intentional. Java/Groovy Annotations such as @Library must be applied to an element.
 * That is often a using statement, but that isn’t needed here so by convention we use an _.
 */

pipeline {

  options {
      disableConcurrentBuilds()
  }

  agent any
  
  parameters {
      choice(choices: 'dev\nqa\ncft-qa\ncft-dev', description: 'Which branch to take the backup ?', name: 'BACKUP_ENV_CHOICE')
  }

  stages {
    stage('Running groovy script'){
      steps {
        script {
          currentBuild.description = "backup: ${params.BACKUP_ENV_CHOICE}"
          inject_environment("${params.BACKUP_ENV_CHOICE}")
          db_backup_runtask()
        }
      }
    }
  }

  post {
    always {
      echo 'One way or another, I have finished'
      deleteDir()
    }
    success {
      script {
        if (env.WEBHOOK_URL)
        {
          office365ConnectorSend message:"success ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", status:"SUCCESS", webhookUrl:"$WEBHOOK_URL", color: "009900"
        }
      }
    }
    failure {
      script {
        if (env.WEBHOOK_URL)
        {
          office365ConnectorSend message:"failed ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)", status:"FAILED", webhookUrl:"$WEBHOOK_URL", color: "ED1C24"
        }
      }
    }
  }
}

inject_environment(String build_env){
  env.build_environment = build_env
  env.is_lower_environment = 'True'

  switch (build_env) {
    case 'dev':
      env.env_file_starting_letter = 'd'
      break
    case 'qa':
      env.env_file_starting_letter = 'q'
      break
    case 'cft-dev':
      env.env_file_starting_letter = 'd'
      break
    case 'cft-qa':
      env.env_file_starting_letter = 'q'
      break
  }
}

def db_backup_runtask() {
  stage('Stage: database backup')
  {
    sh """
    aws ecs run-task \
      --cluster p-dash-rdsbackup \
      --task-definition rdsbackup-taskdefinitions \
      --launch-type FARGATE \
      --overrides '{"containerOverrides": [{"name": "rdsbackup", "environment": [{ "name": "CLIENT_NAME_BUILD", "value": ${build_environment}},{"name": "LOWER_ENVIRONMENT", "value": ${is_lower_environment}},{"name": "ENV_FILE_START_LETTER","value": ${env_file_starting_letter}}]}]}' \
      --network-configuration "awsvpcConfiguration={subnets=['subnet-0a1807c169d4ba548','subnet-06163ecaf0273e89d','subnet-0922504ca7a88b1f7'],securityGroups=['sg-0d68a6694858e166a'],assignPublicIp='ENABLED'}"
    """
  }
}

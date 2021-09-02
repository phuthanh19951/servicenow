pipeline {
  agent any
  environment {
    APPSYSID = '1206665e1b9e3010ca262f08b04bcb60'
    BRANCH = "${BRANCH_NAME}"
    CREDENTIALS = 'ServiceNow'
    DEVENV = 'https://dev113479.service-now.com/'
    TESTENV = 'https://dev113479.service-now.com/'
    PRODENV = 'https://dev113479.service-now.com/'
    TESTSUITEID = '1206665e1b9e3010ca262f08b04bcb60'
  }
  stages {
    stage('Build') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snApplyChanges(appSysId: "${APPSYSID}", branchName: "${BRANCH}", url: "${DEVENV}", credentialsId: "${CREDENTIALS}")
        snPublishApp(credentialsId: "${CREDENTIALS}", appSysId: "${APPSYSID}", obtainVersionAutomatically: true, url: "${DEVENV}")
      }
    }
    stage('Test') {
      when {
        not {
          branch 'master'
        }
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", appSysId: "${APPSYSID}")
        snRunTestSuite(credentialsId: "${CREDENTIALS}", url: "${TESTENV}", testSuiteSysId: "${TESTSUITEID}", withResults: true)
      }
    }
    stage('Deploy to Prod') {
      when {
        branch 'master'
      }
      steps {
        snInstallApp(credentialsId: "${CREDENTIALS}", url: "${PRODENV}", appSysId: "${APPSYSID}")
      }
    }
  }
}
def gv

pipeline {
  agent any
  // tools {
  //   maven 'Maven'
  //   // Here the value of the tool is Name attribute
  //   // that is given in "global tool configuration"
  // }
  parameters {
    // string(name: 'VERSION', defaultValue: '', description: 'Version to deploy on prod')
    choice(name: 'VERSION', choices: ['1.1.0', '1.2.0', '1.3.0'], description: '')
    booleanParam(name: 'executeTests', defaultValue:true, description: '')
  }
  environment {
    NEW_VERSION = '1.3.0'
    // as parameter we need to pass credential id 
    // which we can get from Jenkins  
  }

  stages {

    stage("init"){
      steps {
        script {
          gv = load "script.groovy"
        }
      }
    }

    stage("build") {
      // echo 'building the application...'
      // echo "building version ${NEW_VERSION}"
      // echo "mvn install" 
      steps {
        script {
          gv.buildApp()
        }
      }
    }
    
    stage("test") {
      when {
        expression {
          params.executeTests == false
        }
      }
      steps {
        script {
          gv.testApp()
        }
      }
      // when {
      //   expression {
      //     BRANCH_NAME == 'dev' || 'master'
      //     // this stage only executes only when current 
      //     // branch name is dev, otherwise it skips the
      //     // stage i.e."test".
      //   }
      // }
      
    }
    stage("deploy") {
      steps {
        script {
          gv.deployApp()
        }
      }
      // withCredentials([
      //   usernamePassword(credentials: 'server-credentials', usernameVariable: USER, passwordVariable: PWD)
      // ]){
      //   echo "some script ${USER} ${PWD}"
      // }
    }
  }

  // Execute some logic AFTER all the stages executed
  // In post block we basically define expressions of either
  //  Build status or Build status changes


  // post {
  //   always {
  //     // 
  //   }
  //   success {
  //     // 
  //   }
  //   failure {
  //     // 
  //   }
  // }
}
pipeline {
  agent any
   parameters {
        string(name: 'environment', defaultValue: 'default', description: 'Workspace/environment file to use for deployment')
        string(name: 'version', defaultValue: '', description: 'Version variable to pass to Terraform')
        booleanParam(name: 'autoApprove', defaultValue: false, description: 'Automatically run apply after generating plan?')
    }
  options {
    skipDefaultCheckout(true)
  }
  stages{
    stage('clean workspace') {
      steps {
        cleanWs()
      }
    }
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('terraform plan') {
      steps {
        script {
               currentBuild.displayName = params.version
            }
            sh './terraformw init -input=false'
            sh './terraformw workspace select ${environment}'
            sh "./terraformw plan -input=false -out tfplan -var 'version=${params.version}' --var-file=environments/${params.environment}.tfvars"
            sh './terraformw show -no-color tfplan > tfplan.txt'
      }
    }
    stage('approval') {
        when {
          not {
           equals expected: true, actual: params.autoApprove
        }
    }
      steps {
                script {
                    def plan = readFile 'tfplan.txt'
                    input message: "Do you want to apply the plan?",
                        parameters: [text(name: 'Plan', description: 'Please review the plan', defaultValue: plan)]
                }
         }
}
stage('terraform') {
      steps {
        sh './terraformw apply -input=false tfplan"'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}

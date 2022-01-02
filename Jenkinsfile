 pipeline {
  agent any
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
        sh './terraformw plan -input=false -out tfplan -no-color'
      }
    }
    stage('approval') {
        steps {
          script {
          def userInput = input(id: 'confirm', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'confirm'] ])
        }
    }
}
    stage('terraform') {
      steps {
        sh './terraformw apply -no-color'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}

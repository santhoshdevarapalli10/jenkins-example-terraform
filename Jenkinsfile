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
    stage('approval') {
        steps {
          script {
          def userInput = input(id: 'yes', message: 'Apply Terraform?', parameters: [ [$class: 'BooleanParameterDefinition', defaultValue: false, description: 'Apply terraform', name: 'yes'] ])
        }
    }
}
    stage('terraform') {
      steps {
        sh './terraformw apply -input=false -no-color'
      }
    }
  }
  post {
    always {
      cleanWs()
    }
  }
}

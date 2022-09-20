#! /usr/bin/env groovy

pipeline {

  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building..'

        sh 'mvn clean package'
        
      }
    }
    stage('Create Container Image') {
      steps {
        echo 'Create Container Image..'
        
        script {
          openshift.withCluster('my cluster') {
            
            openshift.withProject("default") {
                echo "Hello from a non-default project: ${openshift.project()}"
                
            def saSelector = openshift.selector( 'serviceaccount' )
            saSelector.describe()
            //echo "Service account: ${it.name()} is defined in ${openshift.project()}" 
              
            }
          }
        }
      }
    }
  }
}


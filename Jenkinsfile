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
            echo "Hello from ${openshift.cluster()}'s default project: ${openshift.project()}"
            def saSelector = openshift.selector( 'serviceaccount' )
            saSelector.describe()
            //echo "Service account: ${it.name()} is defined in ${openshift.project()}"   
            openshift.withProject("imen") {
                echo "Hello from a non-default project: ${openshift.project()}"
              
            }
          }
        }
      }
    }
  }
}


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
            // Create a Selector capable of selecting all service accounts in mycluster's default project
            def saSelector = openshift.selector( 'serviceaccount' )

            // Prints `oc describe serviceaccount` to Jenkins console
            saSelector.describe()

             // Selectors also allow you to easily iterate through all objects they currently select.
            saSelector.withEach { // The closure body will be executed once for each selected object.
             // The 'it' variable will be bound to a Selector which selects a single
             // object which is the focus of the iteration.
            echo "Service account: ${it.name()} is defined in ${openshift.project()}"   
            openshift.withProject("imen") {
                echo "Hello from a non-default project: ${openshift.project()}"
              
            }
          }
        }
      }
    }
  }
}


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
            
            openshift.withProject() {
                def result = openshift.raw( 'status', '-v' )
                echo "Cluster status: ${result.out}"
              
            }
          }
        }
      }
    }
  }
}


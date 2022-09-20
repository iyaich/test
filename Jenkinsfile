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
            
            openshift.withProject("imen") {
                  def created = openshift.newApp( 'https://github.com/openshift/ruby-hello-worldjson )

                  // This Selector exposes the same operations you have already seen.
                  // (And many more that you haven't!).
                  echo "new-app created ${created.count()} objects named: ${created.names()}"
                  created.describe()

                  // We can create a Selector from the larger set which only selects
                  // the build config which new-app just created.
                  def bc = created.narrow('bc')

                  // Let's output the build logs to the Jenkins console. bc.logs()
                  // would run `oc logs bc/ruby-hello-world`, but that might only
                  // output a partial log if the build is in progress. Instead, we will
                  // pass '-f' to `oc logs` to follow the build until it terminates.
                  // Arguments to logs get passed directly on to the oc command line.
                  def result = bc.logs('-f')

                  // Many operations, like logs(), return a Result object (even a Selector
                  // is a subclass of Result). A Result object contains detailed information about
                  // what actions, if any, took place to accomplish an operation.
                  echo "The logs operation require ${result.actions.size()} oc interactions"

                  // You can even see exactly what oc command was executed.
                  echo "Logs executed: ${result.actions[0].cmd}"

                  // And even obtain the standard output and standard error of the command.
                  def logsString = result.actions[0].out
                  def logsErr = result.actions[0].err

                  // And if after some processing within your pipeline, if you decide
                  // you need to initiate a new build after the one initiated by
                  // new-app, simply call the `oc start-build` equivalent:
                  def buildSelector = bc.startBuild()
                  buildSelector.logs('-f')
              
            }
          }
        }
      }
    }
  }
}


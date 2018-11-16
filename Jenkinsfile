#!/usr/bin/env groovy

// labels for Jenkins node types we will build on
def labels = ['armv7l', 'aarch64']
def builders = [:]
for (x in labels) {
  def label = x // Need to bind the label variable before the closure - can't do 'for (label in labels)'

  // Create a map to pass in to the 'parallel' step so we can fire all the builds at once
  builders[label] = {
    node(label) {
      try {

        stage('build') {
          deleteDir()
          checkout scm
          sh "make"
        }

        stage('test') {
        }

        stage('push') {
          sh "make push"
        }

      } catch(error) {
        throw error

      } finally {
        // Any cleanup operations needed, whether we hit an error or not
      }
    }
  }
}

parallel builders

node('master') {

  try {
    stage('scm') {
      // Clean workspace
      deleteDir()
      // Checkout the app at the given commit sha from the webhook
      checkout scm
    }

    stage('deploy') {
      // Docker deploy
      sh "make deploy"
    }

  } catch(error) {
    throw error

  } finally {
    // Any cleanup operations needed, whether we hit an error or not

  }
}

          

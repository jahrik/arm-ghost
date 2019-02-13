#!/usr/bin/env groovy

ghost_user_pass = 'ghost-user-pass'
ghost_db_host = 'ghost-db-host'
ghost_database = 'ghost-database'

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
        deleteDir()
      }
    }
  }
}

parallel builders

node('master') {

  try {
    stage('scm') {
      deleteDir()
      checkout scm
    }

    stage('deploy') {
      withCredentials([
        usernamePassword(credentialsId: ghost_user_pass,
          usernameVariable: 'DB_USER',
          passwordVariable: 'DB_PASS'),
        string(credentialsId: ghost_db_host,
          variable: 'DB_HOST'),
        string(credentialsId: ghost_database,
          variable: 'DATABASE'),]) {
        echo "DB_USER = ${env.DB_USER}"
        echo "DB_PASS = ${env.DB_PASS}"
        echo "DB_HOST = ${env.DB_HOST}"
        echo "DATABASE = ${env.DATABASE}"
        sh "make deploy"
      }
    }

  } catch(error) {
    throw error

  } finally {
    // Any cleanup operations needed, whether we hit an error or not
    deleteDir()
  }
}

pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''export PATH=$PATH:/opt/kubectl:/usr/local/bin:/opt/maven/bin
env'''
        sh '''cd ${BUILD_NAME}
mvn clean initialize package'''
        sh '''#!/bin/bash

#Do the docker build

#Put kubectl and the aws cli on the path
export PATH=$PATH:/opt/kubectl:/usr/local/bin

cd /var/scripts/${JOB_NAME}
cp /var/lib/jenkins/workspace/${JOB_NAME}/${JOB_NAME}/target/${JOB_NAME}_1.0.0.ear .
./buildImage.sh bw-apps/account ${BUILD_ID}'''
      }
    }

  }
}
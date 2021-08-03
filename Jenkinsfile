pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'env'
        sh '''env
export PATH=$PATH:/opt/kubectl:/usr/local/bin:/opt/maven/bin
cd ${JOB_NAME}
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

    stage('Deploy') {
      steps {
        sh '''#!/bin/bash

#Do the deployment

#Put kubectl and the aws cli on the path
export PATH=$PATH:/opt/kubectl:/usr/local/bin
env
aws eks update-kubeconfig --name anz-presales-eks-paas --region ap-southeast-2
cd /var/scripts/${JOB_NAME}
kubectl apply -f ./bw-apps-account.yaml,./bw-apps-account-service.yaml
kubectl set image deployments/bwce-account bwce-account=911512365270.dkr.ecr.ap-southeast-2.amazonaws.com/bw-apps/account:${BUILD_ID}'''
      }
    }

  }
  environment {
    JOB_NAME = 'Accounts'
  }
}
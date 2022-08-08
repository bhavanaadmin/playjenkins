pipeline {
  agent {
    kubernetes {
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  containers:
  - name: gcloud
    image: gcr.io/google.com/cloudsdktool/cloud-sdk:slim
    command:
    - cat
    tty: true
"""
    }
  }
  stages {
    stage("Pushing Image to GCR") {
      steps {
        container('gcloud') {
          sh "PYTHONUNBUFFERED=1 gcloud builds submit -t  gcr.io/gj-playground/food-app:latest ."
          }
        }
      }
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "web.yaml", kubeconfigId: "3478f74e-fd69-41a6-987d-8083b090850c")
        }
      }
    }
   }
}

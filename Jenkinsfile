/*
IaC :: Create Foodtruck Cluster
This Jenkinsfile creates a Foodtruck cluster (Production, Staging and User)
Parameters needed:
  - "envName": "production", "staging" or "user"
  - "iacRegion": "us-west-1", "us-east-1"
  - Applications: Each application should have an env variable
    - "artifactory" "jcr_ver" "oss_ver"
    - "atlassian"
    - "awx" "_ver"
    - "cmafront"
    - "defectdojo" "_ver"
    - "elk" "_ver"
    - "gitlab"
    - "jenkins" "_ver"
    - "keycloak" "_ver"
    - "mlsecops"
    - "openldap" "_ver"
    - "owaspdeptrack" "_ver"
    - "pca" "_ver"
    - "sde" "_ver"
    - "sde_dev" "_ver" "_admin_mode"
    - "securecodebox"
    - "sonarqube" "_ver"
    - "strapi"
*/

pipeline {
  agent { label 'jenkins-slave' } 
  environment { 
    IMAGE = "406187633378.dkr.ecr.us-east-1.amazonaws.com/foodtruck:latest"
    REGISTRY = "406187633378.dkr.ecr.us-east-1.amazonaws.com"
  }
  stages {
    stage ('Linting') {
      parallel {
        stage ('Ansible') {
          steps {
            script {
              sh /* AWS ECR Auth */ '''aws ecr get-login-password --region us-west-1 | docker login --username AWS --password-stdin $REGISTRY'''
              docker.image("$IMAGE").inside {
                sh /* Linting Ansible using ansible-lint */ '''
                  ansible-lint -x command-instead-of-module,command-instead-of-shell,no-changed-when,unnamed-task,experimental,git-latest -p foodtruck-v1/foodtruck-v1.yml
                '''
              }
            }
          }
        }
      }
    }

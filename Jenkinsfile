pipeline {
    agent {
        kubernetes {
            yaml """
kind: Pod
spec:
  containers:
    - name: kustomize
      image: docker.io/nekottyo/kustomize-kubeval
      tty: true
      command:
      - cat
"""
        }
    }
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage("Checkout SCM") {
            steps {
                checkout scm
            }
        }
        stage("Build Kustomization") {
            steps {
                container("kustomize") {
                    gitStatusWrapper(
                        credentialsId: 'github-access',
                        description: 'Builds the main kustomization.yml to check for valid references and syntax',
                        failureDescription: 'kustomization.yml is not valid',
                        successDescription: 'kustomization.yml is valid',
                        gitHubContext: 'build-kustomization') {
                        sh "kustomize build . > k8s.yml"
                    }
                }
            }
        }
        stage("Check Kubernetes Config validity") {
            steps {
                container("kustomize") {
                    gitStatusWrapper(
                        credentialsId: 'github-access',
                        description: 'Validates the generated kubernetes config',
                        failureDescription: 'kubernetes config is not valid',
                        successDescription: 'kubernetes config is valid',
                        gitHubContext: 'check-k8s'
                    ) {
                        sh "kubeval k8s.yml --strict"
                    }
                }
            }
        }
    }
}

pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "bilal1045/studyai"
        DOCKER_HUB_CREDENTIALS_ID = "dockerhub-token"
        IMAGE_TAG = "v${BUILD_NUMBER}"
    }
    stages {
        stage('Checkout Github') {
            steps {
                echo 'Checking out code from GitHub...'
                // VERIFIED: Using your correct repository URL
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/Bilalbaddi/Automated-AI-Assessment-Platform-with-CI-CD.git']])
            }
        }        
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:${IMAGE_TAG}")
                }
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                script {
                    echo 'Pushing Docker image to DockerHub...'
                    docker.withRegistry('https://registry.hub.docker.com' , "${DOCKER_HUB_CREDENTIALS_ID}") {
                        dockerImage.push("${IMAGE_TAG}")
                    }
                }
            }
        }
        stage('Update Deployment YAML with New Tag') {
            steps {
                script {
                    // Uses double quotes to safely replace the image line
                    sh """
                    sed -i "s|image: .*|image: ${DOCKER_HUB_REPO}:${IMAGE_TAG}|" manifests/deployment.yaml
                    """
                }
            }
        }

        stage('Commit Updated YAML') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'github-token', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                        sh '''
                        git config user.name "Bilalbaddi"
                        git config user.email "bilalbaddi7@gmail.com"
                        git add manifests/deployment.yaml
                        
                        # CRITICAL FIX: Added [skip ci] to stop the infinite loop
                        git commit -m "Update image tag to ${IMAGE_TAG} [skip ci]" || echo "No changes to commit"
                        
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/Bilalbaddi/Automated-AI-Assessment-Platform-with-CI-CD.git HEAD:main
                        '''
                    }
                }
            }
        }
        stage('Install Kubectl & ArgoCD CLI Setup') {
            steps {
                sh '''
                echo 'installing Kubectl & ArgoCD cli...'
                curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
                chmod +x kubectl
                mv kubectl /usr/local/bin/kubectl
                curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
                chmod +x /usr/local/bin/argocd
                '''
            }
        }
        stage('Apply Kubernetes & Sync App with ArgoCD') {
            steps {
                script {
                    // Make sure the Minikube IP (192.168.49.2) is correct for your setup
                    kubeconfig(credentialsId: 'kubeconfig', serverUrl: 'https://192.168.49.2:8443') {
                        sh '''
                        # Ensure the IP 3.83.242.72 is your correct ArgoCD server IP
                        argocd login 3.83.242.72:31704 --username admin --password $(kubectl get secret -n argocd argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d) --insecure
                        argocd app sync sttudy
                        '''
                    }
                }
            }
        }
    }
}
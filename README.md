#Installs the ArgoCD CLI on local machine 
brew install argoc
kubectl create namespace argocd

#deploy ArgoCD components, including its API, UI, and controller) 
#into your Kubernetes cluster
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml

#retrieve the  admin password for ArgoCD and Login
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
[for argocd web] kubectl port-forward svc/argocd-server -n argocd 8081:443   (admin, password from get secret command)
[for argocd cli] argocd login localhost:8081 (admin, password from get secret command)

#verify the login
argocd version

#login via browser 
127.0.0.1:8081


#create image and k8s
docker build -t flask-app .
upload to docker hub


#integrate argocd with github 
#and deploy Kubernetes manifests or Helm
#(when git is updated also container will be updated )
#ArgoCD is a declarative, GitOps-based continuous delivery tool for Kubernetes. It integrates with Git repositories like GitHub to automatically synchronize and deploy Kubernetes manifests whenever changes are made in the Git repository. This ensures that your containerized applications stay updated and aligned with the desired state defined in Git

argocd app create flask-app \
  --repo https://github.com/devopsMR/argocd-example.git \
  --path . \
  --dest-server https://kubernetes.default.svc \
  --dest-namespace [APP-NAME-SPACE] \
  --sync-policy automated

minikube service flask-app-service




***ArgoCD Getting Started***
*Argo CD - Declarative GitOps CD for Kubernetes*

**1. Install Argo CD**

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
This will create a new namespace, argocd, where Argo CD services and application resources will live.


**2. Download Argo CD CLI**
Download the latest Argo CD version from https://github.com/argoproj/argo-cd/releases/latest. More detailed installation instructions can be found via the CLI installation documentation.

Also available in Mac, Linux and WSL Homebrew:

brew install argocd

**3. Access The Argo CD API Server**
By default, the Argo CD API server is not exposed with an external IP. To access the API server, choose one of the following techniques to expose the Argo CD API server:

Service Type Load Balancer
Change the argocd-server service type to LoadBalancer:

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

Ingress
Follow the ingress documentation on how to configure Argo CD with ingress.

Port Forwarding

Kubectl port-forwarding can also be used to connect to the API server without exposing the service.


kubectl port-forward svc/argocd-server -n argocd 8080:443

The API server can then be accessed using https://localhost:8080

**4. Login Using The CLI**
The initial password for the admin account is auto-generated and stored as clear text in the field password in a secret named argocd-initial-admin-secret in your Argo CD installation namespace. You can simply retrieve this password using the argocd CLI:


argocd admin initial-password -n argocd

Using the username admin and the password from above, login to Argo CD's IP or hostname:

argocd login <ARGOCD_SERVER>

Change the password using the command:

argocd account update-password

**5. Create An Application From A Git Repository**
An example repository containing a guestbook application is available at https://github.com/argoproj/argocd-example-apps.git to demonstrate how Argo CD works.

Creating Apps Via CLI
First we need to set the current namespace to argocd running the following command:
kubectl config set-context --current --namespace=argocd

Create the example guestbook application with the following command:


argocd app create guestbook --repo https://github.com/argoproj/argocd-example-apps.git --path guestbook --dest-server https://kubernetes.default.svc --dest-namespace default

**Demo**

![test_2](https://github.com/RomanValkman/AsciiArtify_/assets/131240130/b8a1238c-91f0-4ecb-b017-c25d45f0b822)

# v3 - Ingress

## Simple K8s Django App (will not render css)

### K8s Objects
* ingress
* cluster ip
* deployment

### Run the following commands before running kubectl apply for the first time
` kubectl create secret generic secret-key --from-literal SECRET_KEY=123 `
` kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.3.0/deploy/static/provider/cloud/deploy.yaml `

* may want to restart the local k8s cluster so you avoid errors when trying to run the above command

### Can access the app at http://localhost

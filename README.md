# custom-webhook-k8s
custom webhooks either validating or mutating configurations 


Creating Dockerfile:

docker build -t vinaytalla/webhook-validate:v2 .

docker push  vinaytalla/webhook-validate:v2 .

Creating Dpeloyment of Custom webhook controller written in Python:

kubectl create -f admission-controller-deployment.yaml 

create service for application to listen on port 443:
  
kubectl create -f admission-controller-service.yaml

create a secret to use self-signed certifictes:

kubectl create -f admission-controller-secret.yaml

Generting Self-Signed Certifictes using openssl:

openssl req -x509 -sha256 -newkey rsa:2048 -keyout webhook.key -out webhook.crt -days 365 -nodes -addext "subjectAltName = DNS.1:validate.webhook.svc"

Create Validating Webhook Configuration Object:

kubetl create -f webhook-validating.yaml

Finally to test an sample deployemnt:

kubectl create -f test-validate-webhook.yaml

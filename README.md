# Dependencies
Kubernetes
Minikube
Python
Docker

# Django & Kubernetes & Minikube

Simple project to learn how to use kubernetes and minikube in a django project.

First create a env file inside web folder, I named mine with .env, following this format

```
ENV_ALLOWED_HOST=localhost
DEBUG=1
DJANGO_SUPERUSER_USERNAME=admin
DJANGO_SUPERUSER_PASSWORD=mydjangopw
DJANGO_SUPERUSER_EMAIL=hello@teamcfe.com
DJANGO_SECRET_KEY=raBTOqUfRTTNVj52ia9ZRQsb-KFtukcM8oz0rXbeX-k

POSTGRES_READY=0
POSTGRES_DB=dockerdc
POSTGRES_PASSWORD=mysecretpassword
POSTGRES_USER=myuser
POSTGRES_HOST=localhost
POSTGRES_PORT=5433

REDIS_HOST=redis_db
REDIS_PORT=6380
```

Make sure that your variables of POSTGRES and REDIS matches with the ones defined in docker-compose.yaml.
For run the databases simply run this command.
```
docker compose up -d
```

# Pushing the docker image to docker hub
In order to push the docker image to the docker hub, first it's necessary to build the Dockerfile, with a name and a tag.
```
docker build . -t {username}/{app_name}:{tag} -f Dockerfile
```
Run this command in the same directory as the Dockerfile.

Afterwards, it's possible to run this command.
```
docker push andreapp/django_k8s --all-tags
```

# Running Minikube and kubernetes.
Firstly it's necessary to have the docker hub running, and then start the minikube.
```
minikube start
```
It's important to let the kubernetes know your variables in the environment file, with this command.
```
kubectl create secret generic {name-of-env} --from-env-file=web/.env
```
It's possible to verify the secret with the YAML format with this command.
```
kubectl get secret django-k8s-web-prod-env -o YAML
```
And in order to run the django service, it's only necessary to run the following command.
```
kubectl apply -f .\k8s\apps\django-k8s-web.yaml
```
If there's a necessity to enter inside the kubernetes machine , it's possible running the following command.
```
kubectl exec -it django-k8s-web-deployment-6d6ffd4b7-9xtll -- /bin/bash
```
And to be able to access the service, it's necessary to run this following command.
```
minikube service --all # For all services
```
```
minikube service {service_name}
```
To get to know the service name, it's possible by running the following command.
```
minikube get services
```
If there's no necessity of running the minikube anymore, it's possible to stop the minikube service just by running the following command.
```
minikube delete
```
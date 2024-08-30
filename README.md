# Deploy a Python (Django) web app to Azure App Service - Sample Application

This is the sample Django application for the Azure Quickstart [Deploy a Python (Django or Flask) web app to Azure App Service](https://docs.microsoft.com/en-us/azure/app-service/quickstart-python).  For instructions on how to create the Azure resources and deploy the application to Azure, refer to the Quickstart article.

Sample applications are available for the other frameworks here:

* Flask [https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart](https://github.com/Azure-Samples/msdocs-python-flask-webapp-quickstart)
* FastAPI [https://github.com/Azure-Samples/msdocs-python-fastapi-webapp-quickstart](https://github.com/Azure-Samples/msdocs-python-fastapi-webapp-quickstart)

If you need an Azure account, you can [create one for free](https://azure.microsoft.com/en-us/free/).

## For local development

Fill in a secret value in the `.env` file.

For local development, use this random string as an appropriate value:

```shell
SECRET_KEY=123abc
```

## When you deploy to Azure

For deployment to production, create an app setting, `SECRET_KEY`. Use this command to generate an appropriate value:

```shell
python -c 'import secrets; print(secrets.token_hex())'
```

## How to deploy to AKS

Start Docker Desktop for Windows, and ensure docker engine is "Running".

If you get errors about Hyper-V service or user belonging to docker-users group, just restart the VM.

Ensure you have proper access to the AKS cluster by running the command below.

```
C:\Users\user1234\msdocs-python-django-webapp-quickstart>kubectl cluster-info
Kubernetes control plane is running at https://aks947255-0qn6qdec.041ea12c-d11d-4611-a797-4e0278b1dc3a.privatelink.eastus.azmk8s.io:443
CoreDNS is running at https://aks947255-0qn6qdec.041ea12c-d11d-4611-a797-4e0278b1dc3a.privatelink.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://aks947255-0qn6qdec.041ea12c-d11d-4611-a797-4e0278b1dc3a.privatelink.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

C:\Users\user1234\msdocs-python-django-webapp-quickstart>
```

If AKS access is not OK, run the aks.ps1 powershell script on your desktop, which will configure your vm with the correct access to AKS.

Note that the aks.ps1 script will login to your azure account, so please login once asked to do so.

After aks.ps1 script runs, check again that AKS is OK for you.

Run the following commands to build and deploy the sample application in AKS.

```
az acr login --name <acr-name>.azurecr.io
cd c:\users\<username>\msdocs-python-django-webapp-quickstart
docker build -t <acr-name>.azurercr.io/django-demo .
docker push <acr-name>azurecr.io/django-demo
kubctl apply -f deploy.yaml
kubectl get deployment
kubectl get pods
kubectl get service
kubectl get endpoints
curl http://<endpoint-ip>:8000
```

Open your web browser and go to http://<endpoint-ip>:8000, and you should see the application frontend so you can test it is functional.

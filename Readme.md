# Running Mongo DB container:

Use docker run command to run a mongodb container from the official image which is on the docker hub

    docker run --name mongodb --rm -d -p 27017:27017 mongo

# Dockerizing backend:

    Writing Docker file for the backend to containerize the backend application 

    change directory to backend to build the docker image

    docker build -t goals-backend  .

 creates a docker Image with name goals-backend.

    Run docker container from the image by executing the following command

    docker run --name goals-node --rm -itd -p 80:80 goals-backend  

# Dockerizing Frontend:

    Writing Docker file for the Frontend app to be containerized

    change directory to frontend to build the docker image

    docker build -t goals-frontend  .

 creates a docker Image with name goals-frontend.

    Run docker container from the image by executing the following command

    docker run --name goals-UI --rm -itd -p 3000:3000 goals-frontend  

# Adding network for efficient Cross-Container Communication

A network could be created in docker using :
        
        docker network create <network-name>

    Place all the containers in the same network to see the cross communication across containers happening.






# Connecting Azure Container Registry to store the built Images.

    Create a Container Registry with the name task app and prefix the images with the container registry Name.

    taskapp/samples/goals-backend    latest   
    taskapp/samples/goals-frontend   latest 


    Install AZ-cli in order to connect with Azure 

    https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt


    Authenticate Docker with Azure Container Registry
    az acr login --name <acr_name>

    Tag the Docker image
    docker tag <image_name>:<tag> <acr_name>.azurecr.io/<image_name>:<tag>

    Push the image to Azure Container Registry
    docker push <acr_name>.azurecr.io/<image_name>:<tag>


The Next step would be is to push the docker images to the container registry, so that the Images could be used in further deployments with K8's.


# Connecting Kubernetes (Minikube ) with AZ ACR to use the Images stored for Deployment.

*  Create a Azure Service Principal for authorizing the Kubernetes cluster to pull the Images from the Azure    container Registry.

    Note down the Client ID and the tenant ID for future purposes.

*  Navigate to the certificates and secrets Pane to generate a secret , to be used in the Kubernetes secret Object provide details such as description and select the expiry duration of the secret.

    Note down the Secret ID and Secret Value.

* Assign Required Role to the Service Principal:

    Navigate to your Azure Container Registry resource. Go to "Access control (IAM)" or "Access control (IAM Preview)".Click on "Add role assignment".Assign the "AcrPull" role to the service principal. This role grants pull access to images in the container registry.If you need additional permissions, you can assign other roles like "AcrPush" or "AcrImageSigner".


    To create a secret with necessary permission to pull the Images , a secret needs to be created with the following details:

        kubectl create secret docker-registry <secret-name> \
            --namespace <namespace> \
            --docker-server=<container-registry-name>.azurecr.io \
            --docker-username=<service-principal-ID> \
            --docker-password=<service-principal-password>
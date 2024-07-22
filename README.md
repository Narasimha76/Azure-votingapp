Complete End-to-End CI/CD Process with Azure DevOps and AKS Cluster Deployment
![image](https://github.com/user-attachments/assets/7aa2c672-81b0-447f-b1c7-2e7313574a96)

 



Project Objectives
Application with Microservices:
•	Front-End Web App: Developed using Python, this application allows users to vote between two options.
•	Redis: A data store that collects and manages incoming votes.
•	.NET Worker: A service that processes and stores votes.
•	Postgres Database: A relational database supported by a Docker volume, ensuring persistent data storage.
•	Node.js Web App: Provides real-time updates of voting results through a dynamic web interface.
Implementing Continuous Integration Pipeline (CI):
•	Pipeline Creation: Three separate pipelines were created for the worker, vote and result microservices. Each pipeline automates the build process and is responsible for:
o	Building Docker images from Dockerfiles located in the Azure Repository.
o	Pushing the built Docker images to Azure Container Registry.
Implementing Continuous Deployment (CD):
•	AKS Cluster Setup:
o	An Azure Kubernetes Service (AKS) cluster was established, utilizing Virtual Machine Scale Sets as node pools to handle varying loads.
•	Deployment & Service Configuration:
o	Created YAML files for deployments and services were configured to define the infrastructure and application setup within the AKS cluster.
•	ArgoCD Integration:
o	ArgoCD was installed and configured to automate the deployment process. It synchronizes with the defined configurations to manage application deployments.
•	Shell Script for Image Update:
o	A shell script was created to automatically update the Docker image name with the Build Number whenever a Vote pipeline is triggered. This script ensures that the new Docker image is deployed and replaces the previous version in the AKS cluster.
Implementation:
Step-1: Application with Microservices
The application, consisting of multiple microservices, was sourced from the Docker team's multi-microservice application project. The code was forked from the Docker team's GitHub repository and integrated into my project. The forked code was then pushed into the Azure Repo for further development and deployment.
The application includes:
	A Front-End Web App developed using Python, allowing users to vote between two options.
	Redis, a data store that collects and manages incoming votes.
	A .NET Worker service that processes and stores votes.
	A Postgres Database, a relational database supported by a Docker volume to ensure persistent data storage.
	A Node.js Web App providing real-time updates of voting results through a dynamic web interface.

 

 

Step-2: Implementing Continuous Integration Pipeline (CI)
1) Create Agent Pools: A self-managed agent pool named azure agent was configured in Azure DevOps. This pool manages the build and deployment tasks, providing dedicated resources for executing the CI/CD pipelines.
 
 
Steps to Create a Self-Hosted Agent Pool in Azure DevOps
•	Sign in to Azure DevOps: Navigate to Azure DevOps portal, select your project, and go to project settings.
•	Create a New Agent Pool: Under Pipelines, click Agent pools, then Add pool, name it, and click Create.
•	Download the Agent Package: In agent pool details, click New agent, select your OS, and download the agent package.
•	Configure the Agent on Your Laptop: Extract the package, run the config script, and follow prompts for URL, PAT, and agent details.
•	Install and Start the Agent: Run the run script to start the agent, optionally install as a service using svc commands.
•	Verify in Azure DevOps: Go back to Azure DevOps portal, select your agent pool, and verify the agent is listed and online.

 

2) Create Three Pipelines: Separate pipelines were established for the worker, vote and result microservices. Each pipeline is dedicated to automating the build and push processes for its respective service. 

 

Automated Build Process: Each pipeline is designed to build Docker images from Dockerfiles located in the Azure Repository. This ensures that the latest code changes are compiled into a Docker image automatically.
 
Automated Push Process: Once the Docker images are built, the pipelines automatically push these images to the Azure Container Registry. This step is crucial for maintaining an up-to-date image repository.
 


Vote Pipeline: Includes an additional Update stage to update deployments with the new image.
 
Step-3: Implementing Continuous Deployment (CD):
1) AKS Cluster Setup: An Azure Kubernetes Service (AKS) cluster was established to manage and deploy containerized applications. Virtual Machine Scale Sets were used as node pools to dynamically scale the cluster based on the workload.
 
 

 

2) Deployment and Service Configuration: YAML files were created and configured for deployments and services within the AKS cluster. These files define the desired state and configuration of the infrastructure and applications, ensuring a consistent deployment environment.
•	Deployment YAML files specify the desired state for each microservice, including the container image, replicas, and update strategies. They ensure that the correct version of the application is deployed with the desired number of replicas for high availability.
•	Service YAML files define how each microservice is exposed within the AKS cluster. They specify the type of service (e.g., ClusterIP, LoadBalancer), port mappings, and selectors to route traffic to the appropriate pods, ensuring seamless communication between services and external access when needed.
 

3) Install and configure the ArgoCD: ArgoCD was installed and configured to automate and manage the deployment process. It continuously monitors the Git repository for changes and synchronizes the defined configurations to maintain the desired application state within the AKS cluster.
Steps to install and configure the Argocd
•	Install ArgoCD: Deploy ArgoCD to your AKS cluster by applying the official ArgoCD installation manifest. This sets up the necessary components within a dedicated namespace, ensuring a clean and isolated environment for ArgoCD.
 
•	Create and Expose ArgoCD Namespace: Create a namespace for ArgoCD (e.g., argocd) to manage its resources. Expose ArgoCD services by configuring either a LoadBalancer service or an Ingress resource, making the ArgoCD UI accessible externally.
 
 

•	Access ArgoCD UI: Obtain the initial admin password and use it to log in to the ArgoCD web UI. This allows you to manage and configure ArgoCD applications through its user interface.
•	Add The inbound security rule for the AKS Agent pool to access the ArgoCD UI 
 
•	Now Access the ArgoCD using : <IP of the machine>:<Argocd-Server Port>
 
•	Get the password from the secrets
 

•	Connect to GIT Repository: Configure ArgoCD to connect to your Git repository where Kubernetes manifests are stored. This enables ArgoCD to access, monitor, and synchronize your application's deployment configurations from the repository.
•	Create ArgoCD Applications: Define ArgoCD applications to represent your deployments. Set up these applications to specify the source repository and path for your Kubernetes manifests and configure the target namespace within your AKS cluster.
 
  	
 
Testing the project:
Modify Code in GitHub:
•	Start by making changes to the application code in the GitHub repository. These changes could be updates to functionality, bug fixes, or new features.
 

Triggers CI Pipeline:
•	The code changes automatically trigger the Continuous Integration (CI) pipeline. This pipeline builds a new Docker image with the updated code and pushes it to the Azure Container Registry.
 
New image will be stored in the azure Container Registry 
 
Updates Deployment Manifests:
•	A shell script updates the Kubernetes manifest files with the new Docker image details. This ensures that the AKS deployment configurations reflect the latest image.
 
 
Auto Deploys with ArgoCD:
•	ArgoCD detects changes in the Git repository and synchronizes the updated manifests. It then deploys the new image to the AKS cluster, ensuring that the latest version of the application is live.
 
Then the new Applications will be deployed in the AKS cluster, and new application can be accessed through accessing the IP with port.  







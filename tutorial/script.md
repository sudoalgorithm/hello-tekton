# Prerequisites

**FOR THE FAST START LAB, AN OPENSHIFT CLUSTER WILL BE PROVIDED FOR YOU IF YOU DO NOT ALREADY HAVE ONE.** 

1. You must have an **IBM Cloud account**. The account is free and provides access to everything that you need to develop, track, plan, and deploy apps. <a href="https://cloud.ibm.com/registration" target="_blank">Sign up for a trial</a>. The account requires an IBMid. If you don't have an IBMid, you can create one when you register.

2. You need a Managed Red Hat OpenShift cluster. You can create it by using either the UI or the command-line interface (CLI).

   a. Go to the <a href="https://cloud.ibm.com/catalog?category=containers&search=kubernetes" target="_blank">Container Services catalog entry</a>. 

   b. Click **Red Hat OpenShift Cluster**.
   
   c. On the Red Hat OpenShift Cluster page, click **Create**. On the "Create a new OpenShift cluster" page, select the options you would like, and click **Create Cluster**. For more information, see the <a href="https://cloud.ibm.com/docs/openshift?topic=openshift-getting-started" target="_blank">IBM Cloud documentation</a>. 

3. You will need to identify the **OpenShift Cluster URL** and provide a **security token** for access to the cluster during the tutorial. To access your cluster:

   a. From the left menu, select **OpenShift**.

   b. Click on your cluster name to access the cluster dashboard.

   c. Click the **OpenShift web console** button.

   d. Click on your login profile in the top right corner, and select **Copy login command**. Paste this command into a text editor and save it, you will need the URL and token contained in the command later. The login command should look like this: 
   
       oc login <clusterURL> --token=<clusterToken>

4. You need to create an **IBM Cloud API key**. To create a key, go to Manage-->Access--><a href="https://cloud.ibm.com/iam/apikeys?cm_mmc=IBMBluemixGarageMethod-_-MethodSite-_-10-19-15::12-31-18-_-api-keys" target="_blank">IBM Cloud API keys</a> and click **Create an IBM Cloud API key**. Enter a Name and Description pertaining to the key. 

   **Important**: Save the API key value by either copying or downloading it. You need it when you create your toolchain. For more information about creating a cluster by using the CLI, see the <a href="https://cloud.ibm.com/docs/containers?topic=containers-clusters#clusters_cli" target="_blank">Creating clusters with the IBM Cloud CLI</a> tutorial. For more information about the IBM Cloud Container Registry service, see the <a href="https://cloud.ibm.com/docs/services/Registry?topic=registry-index#index" target="_blank">IBM Cloud documentation</a>.

# Task 1: Create a Toolchain with Tekton Pipelines

In this task, you create a toolchain and add the tools that you need for this tutorial. Before you begin, you need your API key and OpenShift cluster name.

1. Create your toolchain by following one of these options:

   a. Open the toolchain creation page by clicking **Create toolchain**.
   
       <a href="https://cloud.ibm.com/devops/setup/deploy?repository=https%3A%2F%2Fgithub.com%2Fopen-toolchain%2Fempty-toolchain&env_id=ibm:yp:us-east" target="_blank">![Create Toolchain](https://cloud.ibm.com/devops/graphics/create_toolchain_button.png)</a>

   b. Go to <a href="https://cloud.ibm.com/?cm_mmc=IBMBluemixGarageMethod-_-MethodSite-_-10-19-15::12-31-18-_-ibm-bluemix-website" target="_blank">cloud.ibm.com</a>. Open the menu in the upper-left corner and click **DevOps**. Click **Create a Toolchain**. Select **Build Your Own Toolchain**. 
   
      ![Blank Template](./images/Blank_Template.png)
   
   c. If you already have an OpenShift cluster, go to <a href="https://cloud.ibm.com/?cm_mmc=IBMBluemixGarageMethod-_-MethodSite-_-10-19-15::12-31-18-_-ibm-bluemix-website" target="_blank">cloud.ibm.com</a>. Open the menu in the upper-left corner and click **OpenShift**. Click **Clusters** and click your cluster. On the cluster dashboard, click the **DevOps** tab and click **Create a Toolchain**. Select **Build Your Own Toolchain**. 
   
**Tip:** For instructions to navigate to the toolchain templates and select a toolchain to create, see Navigating to the toolchain templates.

On the "Build your own toolchain" page, review the default information for the toolchain settings. The toolchain's name identifies it in IBM Cloud. Make sure that the toolchain's name is unique within IBM Cloud. Each toolchain is associated with a specific region and resource group. From the menus on the page, select the region and resource group where you want to create the toolchain. You can have up to 200 toolchains per resource group. 


Note: In order to use the IBM Managed pipeline worker, you must select Dallas as the region for the toolchain.


If you want to switch to a different account, click the profile avatar icon on the banner and select the account. For the purposes of this lab, make sure the Dallas region is selected.


3.  Click Create. The blank toolchain is created.
4 .  Click Add a Tool and click Git Repos and Issue Tracking.   
From the Repository type list, select Clone.
In the Source repository URL field, type https://github.com/stevenjweaver/hello-tekton.
Uncheck the Make this repository private checkbox, and Check the Track deployment of code changes checkbox.  
Click Create Integration. Tiles for Git Issues and Git Code are added to your toolchain.
5.   Return to your toolchain's overview page.


Note: For this tutorial, we will be using the managed pipeline worker provided by the Continuous Delivery service in Dallas. If you would prefer to create your own private pipeline worker, you can follow the steps in the Optional Task 3 below.


6.   Click Add a Tool and click Delivery Pipeline.
a. Type a name for your new pipeline.
b. Click Tekton.   c. Make sure that the Show apps in the View app menu checkbox is selected. All the apps that your pipeline creates are shown in the View App list on the toolchain's Overview page.
d. Click Create Integration to add the Delivery Pipeline to your toolchain.
11. Click the Delivery Pipeline tile to open the Tekton Delivery Pipeline dashboard. The Configure Pipeline page will open. Select the Definitions tab and click Add to select your repository:
Specify the Git repo and URL that contains the Tekton pipeline definition and related artifacts. From the list, select the Git repo that you created earlier.
Select the branch in your Git repo that you want to use. For this tutorial, use the default value.
Specify the path to your pipeline definition within the Git repo. You can reference a specific definition within the same repo. For this tutorial, use the default value.  
Click Save.
12. Click the Worker tab and select the "IBM Managed workers in DALLAS" pipeline worker. If you are using a private pipeline worker, select the one that you want to use to run your Tekton pipeline on the associated cluster.   
13. Click the Triggers tab and click Add trigger and click Git Repository. Associate the trigger with an event listener: 
From the Repository list, select your repo.
Select the When a commit is pushed checkbox, and make sure that listener is selected in the EventListener field.
Click Save.  
14. On the Triggers tab, click Add trigger and click Manual. Associate that trigger with an event listener:
Make sure that listener is selected in the EventListener field.
Click Save.  Note: Manual triggers run when you click Run pipeline and select the trigger. Git Repository triggers run when the specified Git event type occurs for the specified Git repo and branch. The list of available event listeners is populated with the listeners that are defined in the pipeline code repo. 
15. Click the Environment properties tab and define the environment properties for this tutorial. To add each property, click Add property and click Text property. Add these properties:
apikey: Type the API key that you created earlier in this tutorial.
cluster: Type the name of the OpenShift cluster that you created.
clusterURL: Type the URL of the Openshift cluster that you saved when you copied the OpenShift login command.
clusterToken: Copy and paste the token that you saved when you copied the OpenShift login command.
clusterNamespace: Type the namespace in your cluster where the app will be deployed. The default is prod.
clusterRegion: Type the region where your OpenShift cluster is located. The default is us-south.
registryNamespace: Type the IBM Cloud Container Registry namespace where the app image will be built and stored. To use an existing namespace, use the CLI and run ibmcloud cr namespace-list to identify all your current namespaces. 
registryRegion: Type the region where your IBM Cloud Container Registry is located. The default is US-South. To find your registry region, use the CLI and run ibmcloud cr region.
repository: Type the source Git repository where your resources are cloned. The default is https://github.com/open-toolchain/hello-tekton. Change this value if you're forking this repo. 


FOR FAST START, USE THESE VARIABLES: https://ibm.box.com/s/01tbzg1g7reeotcplx2l403ryhy2gz83 


16. Click Save and click Close. Your toolchain is now set up.


Task 2: Explore the pipeline


With a Tekton-based delivery pipeline, you can automate the continuous building, testing, and deployment of your apps. The Tekton Delivery Pipeline dashboard displays an empty table until at least one Tekton pipeline runs. After a Tekton pipeline runs, either manually or as the result of external Git events, the table lists the run, its status, and the last updated time of the run definition.


To run the manual trigger that you set up in the previous task, click Run pipeline and select the name of the manual trigger that you created. The pipeline starts to run and you can see the progress on the dashboard. 


Pipeline runs can be in any of the following states:
Pending: The PipelineRun definition is queued and waiting to run.
Running: The PipelineRun definition is running in the cluster.
Succeeded: The PipelineRun definition was successfully completed in the cluster.
Failed: The PipelineRun definition run failed. Review the log file for the run to determine the cause.  
For more information about a selected run, click any row in the table. You view the Task definition and the steps in each PipelineRun definition. You can also view the status, logs, and details of each Task definition and step, and the overall status of the PipelineRun definition.
  


The pipeline definition is stored in the pipeline.yaml file in the .tekton folder of your Git repository. Each task has a separate section of this file. The steps for each task are defined in the tasks.yaml file.
Review the pipeline-build-task. The task consists of a git clone of the repository followed by two steps:
pre-build-check: This step checks for the mandatory Dockerfile and runs a lint tool. It then checks the registry current plan and quota before it creates the image registry namespace if needed.
build-docker-image: This step creates the Docker image by using the IBM Cloud Container Registry build service through the ibmcloud cr build CLI script. The script stores the output image into the private IBM Cloud Container Registry and copies other app scripts with the build results into the ARCHIVE_DIR folder.
Review the pipeline-validate-task. The task consists of a git clone of the repository, followed by the check-vulnerabilities step. This step runs the IBM Cloud Vulnerability Advisor on the image to check for known vulnerabilities. If it finds a vulnerability, the job fails, preventing the image from being deployed. This safety feature prevents apps with security holes from being deployed. The image has no vulnerabilities, so it passes. In this tutorial template, the default configuration of the job is to not block on failure.
Review the pipeline-deploy-task. The task consists of a git clone of the repository followed by two steps:
pre-deploy-check: This step checks whether the IBM Container Service cluster is ready and has a namespace that is configured with access to the private image registry by using an IBM Cloud API Key. This step also configures the Helm Tiller service to later run a deployment with Helm.
deploy-to-kubernetes: This step checks for cluster readiness and namespace existence, configures the cluster namespace, updates the deployment.yml manifest file, and grants access to the private image registry. It also sets environment variables and uses the deployment manifest file to deploy the app into the Kubernetes cluster.
After all the steps in the pipeline are completed, a green status is shown for each task. Click the deploy-to-kubernetes step and click the Logs tab to see the successful completion of this step.  
Scroll to the end of the log. The DEPLOYMENT SUCCEEDED message is shown at the end of the log.  
Click the URL to see the running application.  


Task 3 (Optional): Private Pipeline Workers


Note that for this tutorial, we are using the Managed Pipeline Worker provided by the Continuous Delivery service in Dallas. If you have a cluster that is not accessible via the public network, you need to use a Private Pipeline worker. See the steps below to add a worker to your toolchain.


Click Add a Tool and click Delivery Pipeline Private Worker. Private pipeline workers are the entities that run the pipeline stages. Typically, workers are used to access information that isn't publicly available. For example, you might use them to adhere to a security policy where jobs need to be run in non-public environments or to access a private repository. For more information about private workers, see Working with Delivery Pipeline Private Workers.   
Type an integration name for the worker.
Create a Service ID API key by clicking Create next to the field. Type a name and description for the worker and click Create.  c. Click Create Integration.
Configure the pipeline worker: 
From the CLI, install the Delivery Pipeline Kubernetes Private Worker support:   
kubectl apply --filename "https://private-worker-service.us-south.devops.cloud.ibm.com/install"
b.   Register a new worker in your cluster (example below. Copy the command provided in the Private Pipeline Worker "Getting Started" Section):   
kubectl apply --filename "https://private-worker-service.us-south.devops.cloud.ibm.com/install/worker?serviceId=ServiceId-74efc386-0dda-42fb-ac73-7a433935872a&apikey=fcYkUis5kpX7QeMRQbDE9MVlwysjwGR0ntKVGkHFRBbL&name=myk8scluster"
c.   Verify that the worker was created:
kubectl get workeragent
Note: You can also run these commands from your OpenShift cluster's web terminal. To access the web terminal, in IBM Cloud, open the menu in the upper-left corner and click OpsnShift. Click Clusters and click your cluster. In the upper-right corner, click Web Terminal and copy the commands into the new terminal window.
8.   To verify that the private pipeline worker is configured correctly, click Delivery Pipeline Private Worker.
Make sure that your registered worker is in the list:  
9.   Return to your toolchain's overview page.

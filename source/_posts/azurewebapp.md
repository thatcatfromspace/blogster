---
title:  Hosting your webapp on Azure in 5 minutes
---

I was recently learning various Azure services for my internship and stumbled upon Azure App Service, a one-shop solution for hosting webapps.

Hosting your app on Azure App Service is the easiest way to hosting. It automatically handles traffic and scaling.

## Deployment slots

Using the Azure portal, you can easily add **deployment slots** to an App Service web app. For instance, you can create a **staging** deployment slot where you can push your code to test on Azure. Once you're happy with your code, you can easily **swap** the staging deployment slot with the production slot.

![Screenshot of the staging deployment slot to test the deployments.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/2-deployment-slots.png)


### CI/CD support

Azure portal provides out of the box support for:

- Azure Repos
- GitHub
- BitBucket
- Local Git

Additionally, manual deployment with FTP is also possible (push/sync).


### Integrated VS and FTP publishing

### Built-in autoscale support (automatic scale-out based on real-world load)

The ability to scale up/down or scale out is baked into the web app. Depending on the web app's usage, you can scale your app up/down by increasing/decreasing the resources of the underlying machine that's hosting your web app. Resources can be the number of cores or the amount of RAM available. On the other hand, you can scale out your app by increasing the number of machine instances that are running your web app.


## Creating the web app

1. **Create a Web App resource.** Creating one allocates a set of resources in App Service.
	- While creating the resource, we need to choose a publish type (code/Docker container) and selecting the Container activates inputs for the associated Docker registry.
	- Runtime stack needs to be provided if hosted as code. Docker containers come with the information included.

2. **Choose an operating system.** If the app is being deployed as code, many runtime stacks are limited to one OS. If app is in a container, specify the OS in the container (in the Dockerfile).

3. **Creating an App Service plan.** An App Service plan is a set of virtual server resources that run App Service apps. 
	- A plan's size (sku/pricing tier) determines the performance. 
	- All App Service apps must be associate with ONE App Service plan.
	- A single plan *can* host multiple apps, but performance varies. Billing is solely based on size of plan and bandwidth resources used. 
	- When using the Azure portal, you will be prompted to create a new plan when creating an app, if one doesn't already exist.


![Screenshot showing web app creation details.](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/media/3-create-web-app-node.png)

NOTES:

- The app's name must be unique from all other app names - this will be used in the URL of the deployed website, for example. an app name `mywebsite` yields a URL of `mywebsite.azurewebsites.net`


## Previewing the web app

1. When deployment is complete, select `Go to resource`. The portal shows the App Service Overview pane for the web app.
2. Select the default URL.


The next step is readying the code for the actual deployment. This is assuming that the code is fully ready and functional. 

## Deploy code to App Service

### Automated deployment 

Continuous integration. As discussing in CI/CD support, the App Service supports several version control systems. 
For GitHub, once the repo is connected to Azure, any push to the production branch is automatically deployed.

Note: Manual deployment is not discussed here. [Look here](https://learn.microsoft.com/en-us/training/modules/host-a-web-app-with-azure-app-service/6-deploying-code-to-app-service) for the resource.


<properties
	pageTitle="Best Practices for Azure App Service"
	description="Learn best practices and troubleshooting for Azure App Service."
	services="app-service"
	documentationCenter=""
	authors="dariagrigoriu"
	manager="wpickett"
	editor="mollybos"/>

<tags
	ms.service="app-service"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/19/2016"
	ms.author="dariagrigoriu"/>
    
# Best Practices for Azure App Service

This article summarizes best practices for using [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). 

## <a name="colocation"></a>Colocation
When Azure resources composing a solution such as a web app and a database are located in different regions the effects can include the following:

*  Increased latency in communication between resources
*  Monetary charges for outbound data transfer cross-region as noted on the [Azure pricing page](https://azure.microsoft.com/pricing/details/data-transfers).

Colocation in the same region is best for Azure resources composing a solution such as a web app and a database or storage account used to hold content or data. When creating resources you should make sure they are in the same Azure region unless you have specific business or design reason for them not to be. You can move an App Service app to the same region as your database by leveraging the [App Service cloning feature](app-service-web-app-cloning-portal.md) currently available for Premium App Service Plan apps.   

## <a name="memoryresources"></a>When Apps Consume More Memory Than Expected
When you notice an app consumes more memory than expected as indicated via monitoring or service recommendations consider the [App Service Auto-Healing feature](https://azure.microsoft.com/blog/auto-healing-windows-azure-web-sites). One of the options for the Auto-Healing feature is taking custom actions based on a memory threshold. Actions span the spectrum from email notifications to investigation via memory dump to on-the-spot mitigation by recycling the worker process. Auto-healing can be configured via web.config and via a friendly user interface as described at in this blog post for the [App Service Support Site Extension](https://azure.microsoft.com/blog/additional-updates-to-support-site-extension-for-azure-app-service-web-apps).   

## <a name="CPUresources"></a>When Apps Consume More CPU Than Expected
When you notice an app consumes more CPU than expected or experiences repeated CPU spikes as indicated via monitoring or service recommendations consider scaling up or scaling out the App Service plan. If your application is statefull, scaling up is the only option, while if your application is stateless, scaling out will give you more flexibility and higher scale potential. 

For more information about “statefull” vs “stateless” applications you can watch this video: [Planning a Scalable End-to-End Multi-Tier Application on Microsoft Azure Web App](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DEV-B414#fbid=?hashlink=fbid). For more information about App Service scaling and autoscaling options read: [Scale a Web App in Azure App Service](web-sites-scale.md).  

## <a name="socketresources"></a>When Socket Resources are Exhausted
A common reason for exhausting outbound TCP connections is the use of client libraries which are not implemented to reuse TCP connections, or in the case of a higher level protocol such as HTTP - Keep-Alive not being leveraged. Please review the documentation for each of the libraries referenced by the apps in your App Service Plan to ensure they are configured or accessed in your code for efficient reuse of outbound connections. Also follow the library documentation guidance for proper creation and release or cleanup to avoid leaking connections. While such client libraries investigations are in progress impact may be mitigated by scaling out to multiple instances.  



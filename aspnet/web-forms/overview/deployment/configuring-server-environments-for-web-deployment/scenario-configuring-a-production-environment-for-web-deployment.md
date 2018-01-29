---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
title: "Escenario: Configurar un entorno de producción para la implementación Web | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que debe completar para configurar un proceso similar..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 2e861511-450e-4752-a61e-4a01933f9b6e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/scenario-configuring-a-production-environment-for-web-deployment
msc.type: authoredcontent
ms.openlocfilehash: cdd13f96ddf08ff86b01ef9de17ea82cf038ab28
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="scenario-configuring-a-production-environment-for-web-deployment"></a><span data-ttu-id="fc6dc-103">Escenario: Configurar un entorno de producción para la implementación Web</span><span class="sxs-lookup"><span data-stu-id="fc6dc-103">Scenario: Configuring a Production Environment for Web Deployment</span></span>
====================
<span data-ttu-id="fc6dc-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="fc6dc-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="fc6dc-105">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="fc6dc-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="fc6dc-106">En este tema se describe un escenario de implementación web típica para un entorno de producción y se explica las tareas que debe completar para configurar un entorno similar.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-106">This topic describes a typical web deployment scenario for a production environment and explains the tasks you need to complete in order to set up a similar environment.</span></span>


<span data-ttu-id="fc6dc-107">El entorno de producción es el destino final de una aplicación web o un sitio Web.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-107">The production environment is the final destination for a web application or a website.</span></span> <span data-ttu-id="fc6dc-108">Hasta este momento, la aplicación se ha sometido a pruebas, se ha implementado en un entorno de ensayo y está lista "esté activa."</span><span class="sxs-lookup"><span data-stu-id="fc6dc-108">By this point, your application has been through testing, has been deployed to a staging environment, and is ready to "go live."</span></span> <span data-ttu-id="fc6dc-109">Las características de un entorno de producción pueden variar enormemente según la naturaleza y el propósito de su contenido web, el tamaño de su organización, la audiencia de destino y una gran cantidad de otros factores.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-109">The characteristics of a production environment can vary widely according to the nature and purpose of your web content, the size of your organization, your target audience, and lots of other factors.</span></span> <span data-ttu-id="fc6dc-110">En un escenario de escala empresarial, el entorno de producción puede tener estas características:</span><span class="sxs-lookup"><span data-stu-id="fc6dc-110">In an enterprise-scale scenario, the production environment may have these characteristics:</span></span>

- <span data-ttu-id="fc6dc-111">El entorno consta de varios servidores web con equilibrio de carga y uno o varios servidores de base de datos, a menudo con clústeres de conmutación por error y la creación de reflejo de base de datos.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-111">The environment consists of multiple load-balanced web servers and one or more database servers, often with failover clustering and database mirroring.</span></span>
- <span data-ttu-id="fc6dc-112">Si el entorno está orientado a Internet, es probable que se aísla de su red interna.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-112">If the environment is Internet-facing, it's likely to be segregated from your internal network.</span></span> <span data-ttu-id="fc6dc-113">Es posible en una subred diferente en una red perimetral, es posible en un dominio diferente y es posible en una infraestructura de red completamente diferente.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-113">It may be on a different subnet in a perimeter network, it may be on a different domain, and it may be on an entirely different network infrastructure.</span></span>
- <span data-ttu-id="fc6dc-114">Los desarrolladores y las cuentas de procesos de servidor de compilación son muy poco probable que tenga privilegios de administrador en los servidores de producción.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-114">Developers and build server process accounts are highly unlikely to have administrator privileges on the production servers.</span></span>
- <span data-ttu-id="fc6dc-115">Cambios en las aplicaciones se implementan con menos frecuencia de prueba o implementaciones de ensayo.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-115">Changes to applications are deployed on a less frequent basis than test or staging deployments.</span></span>

> [!NOTE]
> <span data-ttu-id="fc6dc-116">Escalar horizontalmente una implementación de base de datos entre varios servidores queda fuera del ámbito de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-116">Scaling out a database deployment across multiple servers is beyond the scope of this tutorial.</span></span> <span data-ttu-id="fc6dc-117">Para obtener más información sobre esta área, consulte [libros en pantalla de SQL Server](https://technet.microsoft.com/library/ms130214.aspx).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-117">For more information on this area, please consult [SQL Server Books Online](https://technet.microsoft.com/library/ms130214.aspx).</span></span>


<span data-ttu-id="fc6dc-118">Por ejemplo, en nuestro [escenario del tutorial](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), un servidor de Team Build incluye definiciones de compilación que permiten a los usuarios de compilar la solución póngase en contacto con el administrador e implementarlo en un entorno de ensayo en un solo paso.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-118">For example, in our [tutorial scenario](../deploying-web-applications-in-enterprise-scenarios/enterprise-web-deployment-scenario-overview.md), a Team Build server includes build definitions that let users build the Contact Manager solution and deploy it to a staging environment in a single step.</span></span> <span data-ttu-id="fc6dc-119">Cuando la aplicación está lista para implementarse en producción, debido a las restricciones impuestas por los requisitos de seguridad y la infraestructura de red, el administrador del entorno de producción debe copiar el paquete de web en un servidor web de producción e importe manualmente él a través del Administrador de Internet Information Services (IIS).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-119">When the application is ready to be deployed to production, due to the constraints imposed by security requirements and the network infrastructure, the production environment administrator must manually copy the web package onto a production web server and import it through Internet Information Services (IIS) Manager.</span></span>

![](scenario-configuring-a-production-environment-for-web-deployment/_static/image1.png)

## <a name="solution-overview"></a><span data-ttu-id="fc6dc-120">Introducción a la solución</span><span class="sxs-lookup"><span data-stu-id="fc6dc-120">Solution Overview</span></span>

<span data-ttu-id="fc6dc-121">En este escenario, puede deducir estos hechos de un análisis de los requisitos de implementación:</span><span class="sxs-lookup"><span data-stu-id="fc6dc-121">In this scenario, you can deduce these facts from an analysis of the deployment requirements:</span></span>

- <span data-ttu-id="fc6dc-122">Debido a restricciones de seguridad y la configuración de red, no puede configurar el entorno de producción para permitir la implementación de un solo clic o automatizada.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-122">Due to security restrictions and the network configuration, you can't configure the production environment to support one-click or automated deployment.</span></span> <span data-ttu-id="fc6dc-123">Implementación sin conexión es el único enfoque viable en este escenario.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-123">Offline deployment is the only viable approach in this scenario.</span></span>
- <span data-ttu-id="fc6dc-124">El entorno de producción incluye varios servidores web, por lo que puede usar Web Farm Framework (WFF) para crear una granja de servidores.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-124">The production environment includes multiple web servers, so you can use the Web Farm Framework (WFF) to create a server farm.</span></span> <span data-ttu-id="fc6dc-125">Con este enfoque, el administrador sólo tiene que importar la aplicación en un servidor web (el servidor principal) y WFF replicará la implementación en todos los demás servidores de web en el entorno de producción.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-125">Using this approach, the administrator only needs to import the application onto one web server (the primary server), and WFF will replicate the deployment on all the other web servers in the production environment.</span></span>

<span data-ttu-id="fc6dc-126">Estos temas ofrece toda la información que necesita para completar estas tareas:</span><span class="sxs-lookup"><span data-stu-id="fc6dc-126">These topics provide all the information you need in order to complete these tasks:</span></span>

- <span data-ttu-id="fc6dc-127">[Crear una granja de servidores con el marco de trabajo de la granja de servidores Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-127">[Create a Server Farm with the Web Farm Framework](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="fc6dc-128">En este tema se describe cómo crear y configurar una granja de servidores con WFF, para que los componentes y productos de la plataforma web, opciones de configuración y sitios Web y aplicaciones se replican a través de varios servidores web con equilibrio de carga.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-128">This topic describes how to create and configure a server farm using WFF, so that web platform products and components, configuration settings, and websites and applications are replicated across multiple load-balanced web servers.</span></span>
- <span data-ttu-id="fc6dc-129">[Configurar un servidor Web de publicación (implementación sin conexión) de implementación Web](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-129">[Configure a Web Server for Web Deploy Publishing (Offline Deployment)](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md).</span></span> <span data-ttu-id="fc6dc-130">En este tema se describe cómo crear a un servidor web que permite a los administradores importarán e implementación paquetes de web de manualmente, desde una compilación limpia de Windows Server 2008 R2.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-130">This topic describes how to build a web server that lets administrators import and deploy web packages manually, starting from a clean Windows Server 2008 R2 build.</span></span>
- <span data-ttu-id="fc6dc-131">[Configurar un servidor de base de datos de publicación de implementación Web](configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-131">[Configure a Database Server for Web Deploy Publishing](configuring-a-database-server-for-web-deploy-publishing.md).</span></span> <span data-ttu-id="fc6dc-132">En este tema se describe cómo configurar un servidor de base de datos para admitir la implementación, a partir de una instalación predeterminada de SQL Server 2008 R2 y acceso remoto.</span><span class="sxs-lookup"><span data-stu-id="fc6dc-132">This topic describes how to configure a database server to support remote access and deployment, starting from a default installation of SQL Server 2008 R2.</span></span>

## <a name="further-reading"></a><span data-ttu-id="fc6dc-133">Información adicional</span><span class="sxs-lookup"><span data-stu-id="fc6dc-133">Further Reading</span></span>

<span data-ttu-id="fc6dc-134">Para obtener instrucciones acerca de cómo configurar un entorno de prueba de desarrollo típicas, consulte [escenario: configuración de un entorno de prueba para la implementación Web](scenario-configuring-a-test-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-134">For guidance on configuring a typical developer test environment, see [Scenario: Configuring a Test Environment for Web Deployment](scenario-configuring-a-test-environment-for-web-deployment.md).</span></span> <span data-ttu-id="fc6dc-135">Para obtener instrucciones acerca de cómo configurar un entorno típico de almacenamiento provisional, consulte [escenario: configuración de un entorno de ensayo para la implementación Web](scenario-configuring-a-staging-environment-for-web-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="fc6dc-135">For guidance on configuring a typical staging environment, see [Scenario: Configuring a Staging Environment for Web Deployment](scenario-configuring-a-staging-environment-for-web-deployment.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fc6dc-136">[Anterior](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Siguiente](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span><span class="sxs-lookup"><span data-stu-id="fc6dc-136">[Previous](scenario-configuring-a-staging-environment-for-web-deployment.md)
[Next](configuring-a-web-server-for-web-deploy-publishing-remote-agent.md)</span></span>
---
uid: aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
title: Hospedar OWIN en un rol de trabajador de Azure | Documentos de Microsoft
author: MikeWasson
description: "Este tutorial muestra cómo autohospedaje OWIN en un rol de trabajo de Microsoft Azure. Interfaz Web abierta para .NET (OWIN) define una abstracción entre el servidor web. NET..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/11/2014
ms.topic: article
ms.assetid: 07aa855a-92ee-4d43-ba66-5bfd7de20ee6
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/host-owin-in-an-azure-worker-role
msc.type: authoredcontent
ms.openlocfilehash: 8c0fdfdf60ff3bde34b6869adf3f8693b4d9615d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="host-owin-in-an-azure-worker-role"></a><span data-ttu-id="3ba18-104">Host OWIN en un rol de trabajador de Azure</span><span class="sxs-lookup"><span data-stu-id="3ba18-104">Host OWIN in an Azure Worker Role</span></span>
====================
<span data-ttu-id="3ba18-105">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3ba18-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="3ba18-106">Este tutorial muestra cómo autohospedaje OWIN en un rol de trabajo de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-106">This tutorial shows how to self-host OWIN in a Microsoft Azure worker role.</span></span>
> 
> <span data-ttu-id="3ba18-107">[Abrir la interfaz Web para .NET](http://owin.org/) (OWIN) define una abstracción entre servidores web de .NET y las aplicaciones web.</span><span class="sxs-lookup"><span data-stu-id="3ba18-107">[Open Web Interface for .NET](http://owin.org/) (OWIN) defines an abstraction between .NET web servers and web applications.</span></span> <span data-ttu-id="3ba18-108">OWIN separa la aplicación web desde el servidor, lo que hace OWIN ideal para el autohospedaje una aplicación web en su propio proceso, fuera de IIS: por ejemplo, dentro de un rol de trabajador de Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-108">OWIN decouples the web application from the server, which makes OWIN ideal for self-hosting a web application in your own process, outside of IIS–for example, inside an Azure worker role.</span></span>
> 
> <span data-ttu-id="3ba18-109">En este tutorial, aprenderá a autohospedaje una aplicación OWIN dentro de un rol de trabajo de Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-109">In this tutorial, you'll learn how to self-host an OWIN applications inside a Microsoft Azure worker role.</span></span> <span data-ttu-id="3ba18-110">Para obtener más información acerca de los roles de trabajo, consulte [modelos de ejecución de Azure](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span><span class="sxs-lookup"><span data-stu-id="3ba18-110">To learn more about worker roles, see [Azure Execution Models](https://azure.microsoft.com/documentation/articles/fundamentals-application-models/#CloudServices).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3ba18-111">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="3ba18-111">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="3ba18-112">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3ba18-112">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [<span data-ttu-id="3ba18-113">Azure SDK para .NET 2.3</span><span class="sxs-lookup"><span data-stu-id="3ba18-113">Azure SDK for .NET 2.3</span></span>](https://azure.microsoft.com/downloads/)
> - [<span data-ttu-id="3ba18-114">Microsoft.Owin.Selfhost 2.1.0</span><span class="sxs-lookup"><span data-stu-id="3ba18-114">Microsoft.Owin.Selfhost 2.1.0</span></span>](http://www.nuget.org/packages/Microsoft.Owin.SelfHost/2.1.0)


## <a name="create-a-microsoft-azure-project"></a><span data-ttu-id="3ba18-115">Crear un proyecto de Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="3ba18-115">Create a Microsoft Azure Project</span></span>

<span data-ttu-id="3ba18-116">Inicie Visual Studio con privilegios de administrador.</span><span class="sxs-lookup"><span data-stu-id="3ba18-116">Start Visual Studio with administrator privileges.</span></span> <span data-ttu-id="3ba18-117">Se necesitan privilegios de administrador para depurar la aplicación localmente, mediante el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-117">Administrator privileges are needed to debug the application locally, using the Azure compute emulator.</span></span>

<span data-ttu-id="3ba18-118">En el **archivo** menú, haga clic en **New**, a continuación, haga clic en **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-118">On the **File** menu, click **New**, then click **Project**.</span></span> <span data-ttu-id="3ba18-119">De **plantillas instaladas**, en Visual C#, haga clic en **nube** y, a continuación, haga clic en **servicio de nube de Windows Azure**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-119">From **Installed Templates**, under Visual C#, click **Cloud** and then click **Windows Azure Cloud Service**.</span></span> <span data-ttu-id="3ba18-120">Denomine el proyecto "AzureApp" y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-120">Name the project "AzureApp" and click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image2.png)](host-owin-in-an-azure-worker-role/_static/image1.png)

<span data-ttu-id="3ba18-121">En el **nuevo servicio de nube de Windows Azure** cuadro de diálogo, haga doble clic en **rol de trabajo**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-121">In the **New Windows Azure Cloud Service** dialog, double-click **Worker Role**.</span></span> <span data-ttu-id="3ba18-122">Deje el nombre predeterminado ("Roldetrabajo1").</span><span class="sxs-lookup"><span data-stu-id="3ba18-122">Leave the default name ("WorkerRole1").</span></span> <span data-ttu-id="3ba18-123">Este paso agrega un rol de trabajo a la solución.</span><span class="sxs-lookup"><span data-stu-id="3ba18-123">This step adds a worker role to the solution.</span></span> <span data-ttu-id="3ba18-124">Haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-124">Click **OK**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image4.png)](host-owin-in-an-azure-worker-role/_static/image3.png)

<span data-ttu-id="3ba18-125">La solución de Visual Studio que se crea contiene dos proyectos:</span><span class="sxs-lookup"><span data-stu-id="3ba18-125">The Visual Studio solution that is created contains two projects:</span></span>

- <span data-ttu-id="3ba18-126">&quot;AzureApp&quot; define los roles y la configuración de la aplicación de Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-126">&quot;AzureApp&quot; defines the roles and configuration for the Azure application.</span></span>
- <span data-ttu-id="3ba18-127">&quot;WorkerRole1&quot; contiene el código para el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="3ba18-127">&quot;WorkerRole1&quot; contains the code for the worker role.</span></span>

<span data-ttu-id="3ba18-128">En general, una aplicación de Azure puede contener varios roles, aunque en este tutorial usa un solo rol.</span><span class="sxs-lookup"><span data-stu-id="3ba18-128">In general, an Azure application can contain multiple roles, although this tutorial uses a single role.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image5.png)

## <a name="add-the-owin-self-host-packages"></a><span data-ttu-id="3ba18-129">Agregar los paquetes de autohospedaje de OWIN</span><span class="sxs-lookup"><span data-stu-id="3ba18-129">Add the OWIN Self-Host Packages</span></span>

<span data-ttu-id="3ba18-130">Desde el **herramientas** menú, haga clic en **Administrador de paquetes de biblioteca**, a continuación, haga clic en **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-130">From the **Tools** menu, click **Library Package Manager**, then click **Package Manager Console**.</span></span>

<span data-ttu-id="3ba18-131">En la ventana de la consola de administrador de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3ba18-131">In the Package Manager Console window, enter the following command:</span></span>

[!code-console[Main](host-owin-in-an-azure-worker-role/samples/sample1.cmd)]

## <a name="add-an-http-endpoint"></a><span data-ttu-id="3ba18-132">Agregar un extremo HTTP</span><span class="sxs-lookup"><span data-stu-id="3ba18-132">Add an HTTP Endpoint</span></span>

<span data-ttu-id="3ba18-133">En el Explorador de soluciones, expanda el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="3ba18-133">In Solution Explorer, expand the AzureApp project.</span></span> <span data-ttu-id="3ba18-134">Expanda el nodo Roles, haga clic en WorkerRole1 y seleccione **propiedades**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-134">Expand the Roles node, right-click WorkerRole1, and select **Properties**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image6.png)

<span data-ttu-id="3ba18-135">Haga clic en **extremos**y, a continuación, haga clic en **Agregar extremo**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-135">Click **Endpoints**, and then click **Add Endpoint**.</span></span>

<span data-ttu-id="3ba18-136">En el **protocolo** lista desplegable, seleccione "http".</span><span class="sxs-lookup"><span data-stu-id="3ba18-136">In the **Protocol** dropdown list, select "http".</span></span> <span data-ttu-id="3ba18-137">En **puerto público** y **puerto privado**, escriba 80.</span><span class="sxs-lookup"><span data-stu-id="3ba18-137">In **Public Port** and **Private Port**, type 80.</span></span> <span data-ttu-id="3ba18-138">Estos números de puerto pueden ser diferentes.</span><span class="sxs-lookup"><span data-stu-id="3ba18-138">These port numbers can be different.</span></span> <span data-ttu-id="3ba18-139">El puerto público es lo que los clientes utilizan cuando envía una solicitud a la función.</span><span class="sxs-lookup"><span data-stu-id="3ba18-139">The public port is what clients use when they send a request to the role.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image8.png)](host-owin-in-an-azure-worker-role/_static/image7.png)

## <a name="create-the-owin-startup-class"></a><span data-ttu-id="3ba18-140">Crear la clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="3ba18-140">Create the OWIN Startup Class</span></span>

<span data-ttu-id="3ba18-141">En el Explorador de soluciones, haga clic en el proyecto de WorkerRole1 y seleccione **agregar** / **clase** para agregar una nueva clase.</span><span class="sxs-lookup"><span data-stu-id="3ba18-141">In Solution Explorer, right click the WorkerRole1 project and select **Add** / **Class** to add a new class.</span></span> <span data-ttu-id="3ba18-142">Asigne a la clase el nombre `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3ba18-142">Name the class `Startup`.</span></span>

<span data-ttu-id="3ba18-143">Reemplace todo el código reutilizable con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="3ba18-143">Replace all of the boilerplate code with the following:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample2.cs)]

<span data-ttu-id="3ba18-144">El `UseWelcomePage` método de extensión agrega una página HTML sencilla a la aplicación, para comprobar el sitio está en funcionamiento.</span><span class="sxs-lookup"><span data-stu-id="3ba18-144">The `UseWelcomePage` extension method adds a simple HTML page to your application, to verify the site is working.</span></span>

## <a name="start-the-owin-host"></a><span data-ttu-id="3ba18-145">Iniciar el Host OWIN</span><span class="sxs-lookup"><span data-stu-id="3ba18-145">Start the OWIN Host</span></span>

<span data-ttu-id="3ba18-146">Abra el archivo WorkerRole.cs.</span><span class="sxs-lookup"><span data-stu-id="3ba18-146">Open the WorkerRole.cs file.</span></span> <span data-ttu-id="3ba18-147">Esta clase define el código que se ejecuta cuando se inicia y detiene el rol de trabajo.</span><span class="sxs-lookup"><span data-stu-id="3ba18-147">This class defines the code that runs when the worker role is started and stopped.</span></span>

<span data-ttu-id="3ba18-148">Agregue la siguiente instrucción using:</span><span class="sxs-lookup"><span data-stu-id="3ba18-148">Add the following using statement:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample3.cs)]

<span data-ttu-id="3ba18-149">Agregar un **IDisposable** miembro a la `WorkerRole` clase:</span><span class="sxs-lookup"><span data-stu-id="3ba18-149">Add an **IDisposable** member to the `WorkerRole` class:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample4.cs)]

<span data-ttu-id="3ba18-150">En el `OnStart` método, agregue el código siguiente para iniciar el host:</span><span class="sxs-lookup"><span data-stu-id="3ba18-150">In the `OnStart` method, add the following code to start the host:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample5.cs?highlight=5)]

<span data-ttu-id="3ba18-151">El **WebApp.Start** método inicia el host OWIN.</span><span class="sxs-lookup"><span data-stu-id="3ba18-151">The **WebApp.Start** method starts the OWIN host.</span></span> <span data-ttu-id="3ba18-152">El nombre de la `Startup` clase es un parámetro de tipo para el método.</span><span class="sxs-lookup"><span data-stu-id="3ba18-152">The name of the `Startup` class is a type parameter to the method.</span></span> <span data-ttu-id="3ba18-153">Por convención, el host puede llamar el `Configure` método de esta clase.</span><span class="sxs-lookup"><span data-stu-id="3ba18-153">By convention, the host will call the `Configure` method of this class.</span></span>

<span data-ttu-id="3ba18-154">Invalidar el `OnStop` para desechar el  *\_aplicación* instancia:</span><span class="sxs-lookup"><span data-stu-id="3ba18-154">Override the `OnStop` to dispose of the *\_app* instance:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample6.cs)]

<span data-ttu-id="3ba18-155">Este es el código completo para WorkerRole.cs:</span><span class="sxs-lookup"><span data-stu-id="3ba18-155">Here is the complete code for WorkerRole.cs:</span></span>

[!code-csharp[Main](host-owin-in-an-azure-worker-role/samples/sample7.cs)]

<span data-ttu-id="3ba18-156">Compile la solución y presione F5 para ejecutar la aplicación localmente en el emulador de proceso de Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-156">Build the solution, and press F5 to run the application locally in the Azure Compute Emulator.</span></span> <span data-ttu-id="3ba18-157">Dependiendo de la configuración del firewall, deberá permitir que el emulador a través del firewall.</span><span class="sxs-lookup"><span data-stu-id="3ba18-157">Depending on your firewall settings, you might need to allow the emulator through your firewall.</span></span>

<span data-ttu-id="3ba18-158">El emulador de proceso asigna una dirección IP local para el punto de conexión.</span><span class="sxs-lookup"><span data-stu-id="3ba18-158">The compute emulator assigns a local IP address to the endpoint.</span></span> <span data-ttu-id="3ba18-159">Puede encontrar la dirección IP mediante la visualización de la interfaz de usuario del emulador de proceso.</span><span class="sxs-lookup"><span data-stu-id="3ba18-159">You can find the IP address by viewing the Compute Emulator UI.</span></span> <span data-ttu-id="3ba18-160">Haga clic en el icono del emulador en la barra del área de notificación de tareas y seleccione **Mostrar IU del emulador de proceso**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-160">Right-click the emulator icon in the task bar notification area, and select **Show Compute Emulator UI**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image10.png)](host-owin-in-an-azure-worker-role/_static/image9.png)

<span data-ttu-id="3ba18-161">Buscar la dirección IP en implementaciones de servicios de implementación [id], detalles del servicio.</span><span class="sxs-lookup"><span data-stu-id="3ba18-161">Find the IP address under Service Deployments, deployment [id], Service Details.</span></span> <span data-ttu-id="3ba18-162">Abra un explorador web y vaya a http://*dirección*, donde *dirección* es la dirección IP asignada por el emulador de proceso; por ejemplo, `http://127.0.0.1:80`.</span><span class="sxs-lookup"><span data-stu-id="3ba18-162">Open a web browser and navigate to http://*address*, where *address* is the IP address assigned by the compute emulator; for example, `http://127.0.0.1:80`.</span></span> <span data-ttu-id="3ba18-163">Debería ver la página de bienvenida de OWIN:</span><span class="sxs-lookup"><span data-stu-id="3ba18-163">You should see the OWIN welcome page:</span></span>

![](host-owin-in-an-azure-worker-role/_static/image11.png)

## <a name="deploy-to-azure"></a><span data-ttu-id="3ba18-164">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="3ba18-164">Deploy to Azure</span></span>

<span data-ttu-id="3ba18-165">Para este paso, debe tener una cuenta de Azure.</span><span class="sxs-lookup"><span data-stu-id="3ba18-165">For this step, you must have an Azure account.</span></span> <span data-ttu-id="3ba18-166">Si aún no tiene uno, puede crear una cuenta de prueba gratuita en tan solo unos minutos.</span><span class="sxs-lookup"><span data-stu-id="3ba18-166">If you don't already have one, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="3ba18-167">Para obtener más información, consulte [evaluación gratuita de Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="3ba18-167">For details, see [Microsoft Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>

<span data-ttu-id="3ba18-168">En el Explorador de soluciones, haga clic en el proyecto AzureApp.</span><span class="sxs-lookup"><span data-stu-id="3ba18-168">In Solution Explorer, right-click the AzureApp project.</span></span> <span data-ttu-id="3ba18-169">Seleccione **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-169">Select **Publish**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image12.png)

<span data-ttu-id="3ba18-170">Si no inició sesión en su cuenta de Azure, haga clic en **inicio de sesión**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-170">If you are not signed in to your Azure account, click **Sign In**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image14.png)](host-owin-in-an-azure-worker-role/_static/image13.png)

<span data-ttu-id="3ba18-171">Una vez que haya iniciado sesión, elija una suscripción y haga clic en **siguiente**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-171">After you are signed in, choose a subscription and click **Next**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image16.png)](host-owin-in-an-azure-worker-role/_static/image15.png)

<span data-ttu-id="3ba18-172">Escriba un nombre para el servicio de nube y elija una región.</span><span class="sxs-lookup"><span data-stu-id="3ba18-172">Enter a name for the cloud service and choose a region.</span></span> <span data-ttu-id="3ba18-173">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-173">Click **Create**.</span></span>

![](host-owin-in-an-azure-worker-role/_static/image17.png)

<span data-ttu-id="3ba18-174">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3ba18-174">Click **Publish**.</span></span>

[![](host-owin-in-an-azure-worker-role/_static/image19.png)](host-owin-in-an-azure-worker-role/_static/image18.png)

<span data-ttu-id="3ba18-175">La ventana de registro de actividad de Azure muestra el progreso de la implementación.</span><span class="sxs-lookup"><span data-stu-id="3ba18-175">The Azure Activity Log window shows the progress of the deployment.</span></span> <span data-ttu-id="3ba18-176">Cuando se implementa la aplicación, vaya a `http://appname.cloudapp.net/`, donde *appname* es el nombre del servicio en la nube.</span><span class="sxs-lookup"><span data-stu-id="3ba18-176">When the app is deployed, browse to `http://appname.cloudapp.net/`, where *appname* is the name of your cloud service.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ba18-177">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3ba18-177">Additional Resources</span></span>

- [<span data-ttu-id="3ba18-178">Información general del proyecto Katana</span><span class="sxs-lookup"><span data-stu-id="3ba18-178">An Overview of Project Katana</span></span>](an-overview-of-project-katana.md)
- [<span data-ttu-id="3ba18-179">Proyecto de Katana en GitHub</span><span class="sxs-lookup"><span data-stu-id="3ba18-179">Katana Project on GitHub</span></span>](https://github.com/aspnet/AspNetKatana/)
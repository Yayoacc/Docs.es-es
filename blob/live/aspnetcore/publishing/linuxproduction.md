---
title: Hospedar ASP.NET Core en Linux con Nginx
description: "Describe cómo configurar Nginx como un proxy inverso en Ubuntu 16.04 para reenviar el tráfico HTTP a una aplicación web de ASP.NET Core que se ejecuta en Kestrel."
keywords: ASP.NET Core, Linux, nginx, Ubuntu, proxy inverso
author: rick-anderson
ms.author: riande
manager: wpickett
ms.date: 08/21/2017
ms.topic: article
ms.assetid: 1c33e576-33de-481a-8ad3-896b94fde0e3
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/linuxproduction
ms.openlocfilehash: 7c7b949fc922c605aa4554c158200a4123c4eb1c
ms.sourcegitcommit: fc98e93464ccf37d9904e89a71cdddbd4bbdb86a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/05/2018
---
# <a name="set-up-a-hosting-environment-for-aspnet-core-on-linux-with-nginx-and-deploy-to-it"></a><span data-ttu-id="ef5d1-104">Configurar un entorno de hospedaje para ASP.NET Core en Linux con Nginx e implementar en él</span><span class="sxs-lookup"><span data-stu-id="ef5d1-104">Set up a hosting environment for ASP.NET Core on Linux with Nginx, and deploy to it</span></span>

<span data-ttu-id="ef5d1-105">Por [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="ef5d1-105">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="ef5d1-106">En esta guía se explica cómo configurar un entorno de ASP.NET Core listo para producción en un servidor de Ubuntu 16.04.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-106">This guide explains setting up a production-ready ASP.NET Core environment on an Ubuntu 16.04 Server.</span></span>

<span data-ttu-id="ef5d1-107">**Nota:** Para Ubuntu 14.04, se recomienda supervisord como una solución para supervisar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-107">**Note:** For Ubuntu 14.04, supervisord is recommended as a solution for monitoring the Kestrel process.</span></span> <span data-ttu-id="ef5d1-108">systemd no está disponible en Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-108">systemd is not available on Ubuntu 14.04.</span></span> <span data-ttu-id="ef5d1-109">[Vea la versión anterior de este documento](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="ef5d1-109">[See previous version of this document](https://github.com/aspnet/Docs/blob/e9c1419175c4dd7e152df3746ba1df5935aaafd5/aspnetcore/publishing/linuxproduction.md)</span></span>

<span data-ttu-id="ef5d1-110">En esta guía:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-110">This guide:</span></span>

* <span data-ttu-id="ef5d1-111">Se coloca una aplicación existente de ASP.NET Core detrás de un servidor proxy inverso</span><span class="sxs-lookup"><span data-stu-id="ef5d1-111">Places an existing ASP.NET Core application behind a reverse proxy server</span></span>
* <span data-ttu-id="ef5d1-112">Se configura el servidor proxy inverso para reenviar las solicitudes al servidor web de Kestrel</span><span class="sxs-lookup"><span data-stu-id="ef5d1-112">Sets up the reverse proxy server to forward requests to the Kestrel web server</span></span>
* <span data-ttu-id="ef5d1-113">Se asegura que la aplicación web se ejecuta al iniciar como un demonio</span><span class="sxs-lookup"><span data-stu-id="ef5d1-113">Ensures the web application runs on startup as a daemon</span></span>
* <span data-ttu-id="ef5d1-114">Se configura una herramienta de administración de procesos para ayudar a reiniciar la aplicación web</span><span class="sxs-lookup"><span data-stu-id="ef5d1-114">Configures a process management tool to help restart the web application</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ef5d1-115">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="ef5d1-115">Prerequisites</span></span>

1. <span data-ttu-id="ef5d1-116">Acceso a un servidor de Ubuntu 16.04 con una cuenta de usuario estándar con privilegios sudo</span><span class="sxs-lookup"><span data-stu-id="ef5d1-116">Access to an Ubuntu 16.04 Server with a standard user account with sudo privilege</span></span>
2. <span data-ttu-id="ef5d1-117">Una aplicación de ASP.NET Core existente</span><span class="sxs-lookup"><span data-stu-id="ef5d1-117">An existing ASP.NET Core application</span></span>

## <a name="copy-over-your-app"></a><span data-ttu-id="ef5d1-118">Copiar a través de la aplicación</span><span class="sxs-lookup"><span data-stu-id="ef5d1-118">Copy over your app</span></span>

<span data-ttu-id="ef5d1-119">Ejecute `dotnet publish` desde el entorno de desarrollo para empaquetar una aplicación en un directorio independiente que se puede ejecutar en el servidor.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-119">Run `dotnet publish` from the dev environment to package an app into a self-contained directory that can run on the server.</span></span>

<span data-ttu-id="ef5d1-120">Copie la aplicación de ASP.NET Core en el servidor mediante cualquier herramienta (SCP, FTP, etc.) que se integre en el flujo de trabajo.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-120">Copy the ASP.NET Core app to the server using whatever tool (SCP, FTP, etc.) integrates into your workflow.</span></span> <span data-ttu-id="ef5d1-121">Pruebe la aplicación, por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-121">Test the app, for example:</span></span>
 - <span data-ttu-id="ef5d1-122">Ejecute `dotnet yourapp.dll` desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-122">From the command line, run `dotnet yourapp.dll`</span></span>
 - <span data-ttu-id="ef5d1-123">En un explorador, vaya a `http://<serveraddress>:<port>` para comprobar que la aplicación funciona en Linux.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-123">In a browser, navigate to `http://<serveraddress>:<port>` to verify the app works on Linux.</span></span> 
 
## <a name="configure-a-reverse-proxy-server"></a><span data-ttu-id="ef5d1-124">Configurar un servidor proxy inverso</span><span class="sxs-lookup"><span data-stu-id="ef5d1-124">Configure a reverse proxy server</span></span>

<span data-ttu-id="ef5d1-125">Un proxy inverso es una configuración común para usar aplicaciones web dinámicas.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-125">A reverse proxy is a common setup for serving dynamic web applications.</span></span> <span data-ttu-id="ef5d1-126">Un proxy inverso finaliza la solicitud HTTP y la reenvía a la aplicación de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-126">A reverse proxy terminates the HTTP request and forwards it to the ASP.NET Core application.</span></span>

### <a name="why-use-a-reverse-proxy-server"></a><span data-ttu-id="ef5d1-127">¿Por qué usar un servidor proxy inverso?</span><span class="sxs-lookup"><span data-stu-id="ef5d1-127">Why use a reverse proxy server?</span></span>

<span data-ttu-id="ef5d1-128">Kestrel es una herramienta excelente para usar contenido dinámico de ASP.NET Core; con todo, los elementos de servicio web no tienen tantas características como los servidores como IIS, Apache o Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-128">Kestrel is great for serving dynamic content from ASP.NET Core; however, the web serving parts aren’t as feature rich as servers like IIS, Apache, or Nginx.</span></span> <span data-ttu-id="ef5d1-129">Un servidor proxy inverso puede descargar trabajo (por ejemplo, usar contenido estático, almacenar solicitudes en caché, comprimir solicitudes y finalizar SSL desde el servidor HTTP).</span><span class="sxs-lookup"><span data-stu-id="ef5d1-129">A reverse proxy server can offload work like serving static content, caching requests, compressing requests, and SSL termination from the HTTP server.</span></span> <span data-ttu-id="ef5d1-130">Un servidor proxy inverso puede residir en un equipo dedicado o se puede implementar junto con un servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-130">A reverse proxy server may reside on a dedicated machine or may be deployed alongside an HTTP server.</span></span>

<span data-ttu-id="ef5d1-131">Para los fines de esta guía, se usa una única instancia de Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-131">For the purposes of this guide, a single instance of Nginx is used.</span></span> <span data-ttu-id="ef5d1-132">Se ejecuta en el mismo servidor, junto con el servidor HTTP.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-132">It runs on the same server, alongside the HTTP server.</span></span> <span data-ttu-id="ef5d1-133">Según sus requisitos, puede elegir otra configuración.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-133">Based on your requirements, you may choose a different setup.</span></span>

<span data-ttu-id="ef5d1-134">Dado que el proxy inverso reenvía las solicitudes, use el software intermedio `ForwardedHeaders` del paquete `Microsoft.AspNetCore.HttpOverrides`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-134">Because requests are forwarded by reverse proxy, use the `ForwardedHeaders` middleware from the `Microsoft.AspNetCore.HttpOverrides` package.</span></span> <span data-ttu-id="ef5d1-135">El software intermedio actualiza `Request.Scheme`, mediante el encabezado `X-Forwarded-Proto`, para que esos URI de redirección y otras directivas de seguridad funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-135">This middleware updates `Request.Scheme`, using the `X-Forwarded-Proto` header, so that redirect URIs and other security policies work correctly.</span></span>

<span data-ttu-id="ef5d1-136">Al configurar un servidor proxy inverso, el software intermedio de autenticación necesita que se ejecute `UseForwardedHeaders` primero.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-136">When setting up a reverse proxy server, the authentication middleware needs `UseForwardedHeaders` to run first.</span></span> <span data-ttu-id="ef5d1-137">Este orden garantiza que el software intermedio de autenticación puede consumir los valores afectados y generar URI de redirección correctos.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-137">This ordering ensures that the authentication middleware can consume the affected values and generate correct redirect URIs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="ef5d1-138">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="ef5d1-138">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="ef5d1-139">Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseAuthentication` o un software intermedio de esquema de autenticación similar:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-139">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseAuthentication();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="ef5d1-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="ef5d1-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="ef5d1-141">Invoque el método `UseForwardedHeaders` (en el método `Configure` de *Startup.cs*) antes de llamar a `UseIdentity` y `UseFacebookAuthentication` o un software intermedio de esquema de autenticación similar:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-141">Invoke the `UseForwardedHeaders` method (in the `Configure` method of *Startup.cs*) before calling `UseIdentity` and `UseFacebookAuthentication` or similar authentication scheme middleware:</span></span>

```csharp
app.UseForwardedHeaders(new ForwardedHeadersOptions
{
    ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
});

app.UseIdentity();
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

### <a name="install-nginx"></a><span data-ttu-id="ef5d1-142">Instalar Nginx</span><span class="sxs-lookup"><span data-stu-id="ef5d1-142">Install Nginx</span></span>

```bash
sudo apt-get install nginx
```

> [!NOTE]
> <span data-ttu-id="ef5d1-143">Si planea instalar los módulos opcionales de Nginx, es posible que deba compilar Nginx desde el origen.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-143">If you plan to install optional Nginx modules, you may be required to build Nginx from source.</span></span>

<span data-ttu-id="ef5d1-144">Use `apt-get` para instalar Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-144">Use `apt-get` to install Nginx.</span></span> <span data-ttu-id="ef5d1-145">El instalador crea un script de inicio de System V que ejecuta Nginx como demonio al iniciarse el sistema.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-145">The installer creates a System V init script that runs Nginx as daemon on system startup.</span></span> <span data-ttu-id="ef5d1-146">Puesto que Nginx se ha instalado por primera vez, ejecute lo siguiente para iniciarlo de forma explícita:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-146">Since Nginx was installed for the first time, explicitly start it by running:</span></span>

```bash
sudo service nginx start
```

<span data-ttu-id="ef5d1-147">Compruebe que un explorador muestra la página de aterrizaje predeterminada de Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-147">Verify a browser displays the default landing page for Nginx.</span></span>

### <a name="configure-nginx"></a><span data-ttu-id="ef5d1-148">Configurar Nginx</span><span class="sxs-lookup"><span data-stu-id="ef5d1-148">Configure Nginx</span></span>

<span data-ttu-id="ef5d1-149">Para configurar Nginx como un proxy inverso a fin de que reenvíe solicitudes a nuestra aplicación de ASP.NET Core, modifique `/etc/nginx/sites-available/default`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-149">To configure Nginx as a reverse proxy to forward requests to our ASP.NET Core application, modify `/etc/nginx/sites-available/default`.</span></span> <span data-ttu-id="ef5d1-150">Ábralo en un editor de texto y reemplace el contenido por el siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-150">Open it in a text editor, and replace the contents with the following:</span></span>

```nginx
server {
    listen 80;
    location / {
        proxy_pass http://localhost:5000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection keep-alive;
        proxy_set_header Host $http_host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

<span data-ttu-id="ef5d1-151">Este archivo de configuración de Nginx reenvía tráfico público entrante del puerto `80` al puerto `5000`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-151">This Nginx configuration file forwards incoming public traffic from port `80` to port `5000`.</span></span>

<span data-ttu-id="ef5d1-152">Una vez que haya acabado de realizar cambios en la configuración de Nginx, puede ejecutar `sudo nginx -t` para comprobar la sintaxis de los archivos de configuración.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-152">Once you have completed making changes to your Nginx configuration, you can run `sudo nginx -t` to verify the syntax of your configuration files.</span></span> <span data-ttu-id="ef5d1-153">Si la prueba de los archivos de configuración es correcta, puede pedir a Nginx que recopile los cambios al ejecutar `sudo nginx -s reload`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-153">If the configuration file test is successful, you can ask Nginx to pick up the changes by running `sudo nginx -s reload`.</span></span>

## <a name="monitoring-our-application"></a><span data-ttu-id="ef5d1-154">Supervisar la aplicación</span><span class="sxs-lookup"><span data-stu-id="ef5d1-154">Monitoring our application</span></span>

<span data-ttu-id="ef5d1-155">Nginx ahora está configurado para reenviar las solicitudes realizadas a `http://yourhost:80` en la aplicación de ASP.NET Core que se ejecuta en Kestrel en `http://127.0.0.1:5000`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-155">Nginx is now setup to forward requests made to `http://yourhost:80` on to the ASP.NET Core application running on Kestrel at `http://127.0.0.1:5000`.</span></span> <span data-ttu-id="ef5d1-156">En cambio, Nginx no está configurado para administrar el proceso de Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-156">However, Nginx is not set up to manage the Kestrel process.</span></span> <span data-ttu-id="ef5d1-157">Puede usar *systemd* y crear un archivo de servicio para iniciar y supervisar la aplicación web subyacente.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-157">You can use *systemd* and create a service file to start and monitor the underlying web app.</span></span> <span data-ttu-id="ef5d1-158">*systemd* es un sistema de inicio que proporciona muchas características eficaces para iniciar, detener y administrar procesos.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-158">*systemd* is an init system that provides many powerful features for starting, stopping, and managing processes.</span></span> 

### <a name="create-the-service-file"></a><span data-ttu-id="ef5d1-159">Crear el archivo de servicio</span><span class="sxs-lookup"><span data-stu-id="ef5d1-159">Create the service file</span></span>

<span data-ttu-id="ef5d1-160">Cree el archivo de definición de servicio:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-160">Create the service definition file:</span></span>

```bash
sudo nano /etc/systemd/system/kestrel-hellomvc.service
```

<span data-ttu-id="ef5d1-161">A continuación tiene un archivo de servicio de ejemplo de nuestra aplicación:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-161">The following is an example service file for our application:</span></span>

```ini
[Unit]
Description=Example .NET Web API Application running on Ubuntu

[Service]
WorkingDirectory=/var/aspnetcore/hellomvc
ExecStart=/usr/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
Restart=always
RestartSec=10  # Restart service after 10 seconds if dotnet service crashes
SyslogIdentifier=dotnet-example
User=www-data
Environment=ASPNETCORE_ENVIRONMENT=Production
Environment=DOTNET_PRINT_TELEMETRY_MESSAGE=false

[Install]
WantedBy=multi-user.target
```

<span data-ttu-id="ef5d1-162">**Nota:** Si la configuración no emplea el usuario *www-data*, el usuario definido aquí se debe crear primero y debe proporcionársele la propiedad adecuada para los archivos.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-162">**Note:** If the user *www-data* is not used by your configuration, the user defined here must be created first and given proper ownership for files.</span></span>

<span data-ttu-id="ef5d1-163">Guarde el archivo y habilite el servicio.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-163">Save the file, and enable the service.</span></span>

```bash
systemctl enable kestrel-hellomvc.service
```

<span data-ttu-id="ef5d1-164">Inicie el servicio y compruebe que se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-164">Start the service and verify that it is running.</span></span>

```
systemctl start kestrel-hellomvc.service
systemctl status kestrel-hellomvc.service

● kestrel-hellomvc.service - Example .NET Web API Application running on Ubuntu
    Loaded: loaded (/etc/systemd/system/kestrel-hellomvc.service; enabled)
    Active: active (running) since Thu 2016-10-18 04:09:35 NZDT; 35s ago
Main PID: 9021 (dotnet)
    CGroup: /system.slice/kestrel-hellomvc.service
            └─9021 /usr/local/bin/dotnet /var/aspnetcore/hellomvc/hellomvc.dll
```

<span data-ttu-id="ef5d1-165">Con el proxy inverso configurado y Kestrel administrado a través de systemd, la aplicación web está completamente configurada y se puede tener acceso a ella desde un explorador en el equipo local en `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-165">With the reverse proxy configured and Kestrel managed through systemd, the web application is fully configured and can be accessed from a browser on the local machine at `http://localhost`.</span></span> <span data-ttu-id="ef5d1-166">También es accesible desde un equipo remoto, salvo que haya algún firewall que la pueda estar bloqueando.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-166">It is also accessible from a remote machine, barring any firewall that might be blocking.</span></span> <span data-ttu-id="ef5d1-167">Al inspeccionar los encabezados de respuesta, el encabezado `Server` muestra la aplicación de ASP.NET Core que proporciona Kestrel.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-167">Inspecting the response headers, the `Server` header shows the ASP.NET Core application being served by Kestrel.</span></span>

```text
HTTP/1.1 200 OK
Date: Tue, 11 Oct 2016 16:22:23 GMT
Server: Kestrel
Keep-Alive: timeout=5, max=98
Connection: Keep-Alive
Transfer-Encoding: chunked
```

### <a name="viewing-logs"></a><span data-ttu-id="ef5d1-168">Ver registros</span><span class="sxs-lookup"><span data-stu-id="ef5d1-168">Viewing logs</span></span>

<span data-ttu-id="ef5d1-169">Dado que la aplicación web que usa Kestrel se administra mediante `systemd`, todos los procesos y eventos se registran en un diario centralizado.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-169">Since the web application using Kestrel is managed using `systemd`, all events and processes are logged to a centralized journal.</span></span> <span data-ttu-id="ef5d1-170">En cambio, este diario incluye todas las entradas de todos los servicios y procesos administrados por `systemd`.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-170">However, this journal includes all entries for all services and processes managed by `systemd`.</span></span> <span data-ttu-id="ef5d1-171">Para ver los elementos específicos de `kestrel-hellomvc.service`, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-171">To view the `kestrel-hellomvc.service`-specific items, use the following command:</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service
```

<span data-ttu-id="ef5d1-172">Para obtener más opciones de filtrado, las opciones de tiempo como `--since today`, `--until 1 hour ago` o una combinación de estas pueden reducir la cantidad de entradas que se devuelven.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-172">For further filtering, time options such as `--since today`, `--until 1 hour ago` or a combination of these can reduce the amount of entries returned.</span></span>

```bash
sudo journalctl -fu kestrel-hellomvc.service --since "2016-10-18" --until "2016-10-18 04:00"
```

## <a name="securing-our-application"></a><span data-ttu-id="ef5d1-173">Proteger la aplicación</span><span class="sxs-lookup"><span data-stu-id="ef5d1-173">Securing our application</span></span>

### <a name="enable-apparmor"></a><span data-ttu-id="ef5d1-174">Habilitar AppArmor</span><span class="sxs-lookup"><span data-stu-id="ef5d1-174">Enable AppArmor</span></span>

<span data-ttu-id="ef5d1-175">Linux Security Modules (LSM) es un marco que forma parte del kernel de Linux desde Linux 2.6.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-175">Linux Security Modules (LSM) is a framework that is part of the Linux kernel since Linux 2.6.</span></span> <span data-ttu-id="ef5d1-176">LSM admite diferentes implementaciones de los módulos de seguridad.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-176">LSM supports different implementations of security modules.</span></span> <span data-ttu-id="ef5d1-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) es un LSM que implementa un sistema de control de acceso obligatorio que permite restringir el programa a un conjunto limitado de recursos.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-177">[AppArmor](https://wiki.ubuntu.com/AppArmor) is a LSM that implements a Mandatory Access Control system which allows confining the program to a limited set of resources.</span></span> <span data-ttu-id="ef5d1-178">Asegúrese de que AppArmor está habilitado y configurado correctamente.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-178">Ensure AppArmor is enabled and properly configured.</span></span>

### <a name="configuring-our-firewall"></a><span data-ttu-id="ef5d1-179">Configurar el firewall</span><span class="sxs-lookup"><span data-stu-id="ef5d1-179">Configuring our firewall</span></span>

<span data-ttu-id="ef5d1-180">Cierre todos los puertos externos que no estén en uso.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-180">Close off all external ports that are not in use.</span></span> <span data-ttu-id="ef5d1-181">Uncomplicated Firewall (ufw) proporciona un front-end para `iptables` al proporcionar una interfaz de línea de comandos para configurar el firewall.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-181">Uncomplicated firewall (ufw) provides a front end for `iptables` by providing a command line interface for configuring the firewall.</span></span> <span data-ttu-id="ef5d1-182">Compruebe que `ufw` está configurado para permitir el tráfico en los puertos que necesita.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-182">Verify that `ufw` is configured to allow traffic on any ports you need.</span></span>

```bash
sudo apt-get install ufw
sudo ufw enable

sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

### <a name="securing-nginx"></a><span data-ttu-id="ef5d1-183">Proteger Nginx</span><span class="sxs-lookup"><span data-stu-id="ef5d1-183">Securing Nginx</span></span>

<span data-ttu-id="ef5d1-184">La distribución predeterminada de Nginx no habilita SSL.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-184">The default distribution of Nginx doesn't enable SSL.</span></span> <span data-ttu-id="ef5d1-185">Para habilitar características de seguridad adicionales, compile desde el origen.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-185">To enable additional security features, build from source.</span></span>

#### <a name="download-the-source-and-install-the-build-dependencies"></a><span data-ttu-id="ef5d1-186">Descargar el origen e instalar las dependencias de compilación</span><span class="sxs-lookup"><span data-stu-id="ef5d1-186">Download the source and install the build dependencies</span></span>

```bash
# Install the build dependencies
sudo apt-get update
sudo apt-get install build-essential zlib1g-dev libpcre3-dev libssl-dev libxslt1-dev libxml2-dev libgd2-xpm-dev libgeoip-dev libgoogle-perftools-dev libperl-dev

# Download nginx 1.10.0 or latest
wget http://www.nginx.org/download/nginx-1.10.0.tar.gz
tar zxf nginx-1.10.0.tar.gz
```

#### <a name="change-the-nginx-response-name"></a><span data-ttu-id="ef5d1-187">Cambiar el nombre de la respuesta de Nginx</span><span class="sxs-lookup"><span data-stu-id="ef5d1-187">Change the Nginx response name</span></span>

<span data-ttu-id="ef5d1-188">Edite *src/http/ngx_http_header_filter_module.c*:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-188">Edit *src/http/ngx_http_header_filter_module.c*:</span></span>

```c
static char ngx_http_server_string[] = "Server: Your Web Server" CRLF;
static char ngx_http_server_full_string[] = "Server: Your Web Server" CRLF;
```

#### <a name="configure-the-options-and-build"></a><span data-ttu-id="ef5d1-189">Configurar las opciones y la compilación</span><span class="sxs-lookup"><span data-stu-id="ef5d1-189">Configure the options and build</span></span>

<span data-ttu-id="ef5d1-190">Se necesita la biblioteca PCRE para expresiones regulares.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-190">The PCRE library is required for regular expressions.</span></span> <span data-ttu-id="ef5d1-191">Las expresiones regulares se usan en la directiva de ubicación de ngx_http_rewrite_module.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-191">Regular expressions are used in the location directive for the ngx_http_rewrite_module.</span></span> <span data-ttu-id="ef5d1-192">http_ssl_module agrega compatibilidad con el protocolo HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-192">The http_ssl_module adds HTTPS protocol support.</span></span>

<span data-ttu-id="ef5d1-193">Considere la opción de usar un firewall de aplicaciones web como *ModSecurity* para proteger la aplicación.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-193">Consider using a web application firewall like *ModSecurity* to harden your application.</span></span>

```bash
./configure
--with-pcre=../pcre-8.38
--with-zlib=../zlib-1.2.8
--with-http_ssl_module
--with-stream
--with-mail=dynamic
```

#### <a name="configure-ssl"></a><span data-ttu-id="ef5d1-194">Configurar SSL</span><span class="sxs-lookup"><span data-stu-id="ef5d1-194">Configure SSL</span></span>

* <span data-ttu-id="ef5d1-195">Configure el servidor para que escuche el tráfico HTTPS en el puerto `443`. Para ello, especifique un certificado válido emitido por una entidad de certificados (CA) de confianza.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-195">Configure your server to listen to HTTPS traffic on port `443` by specifying a valid certificate issued by a trusted Certificate Authority (CA).</span></span>

* <span data-ttu-id="ef5d1-196">Refuerce la seguridad con algunos de los procedimientos descritos en el siguiente archivo */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-196">Harden your security by employing some of the practices depicted in the following */etc/nginx/nginx.conf* file.</span></span> <span data-ttu-id="ef5d1-197">Entre los ejemplos se incluye la elección de un cifrado más seguro y el redireccionamiento de todo el tráfico a través de HTTP a HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-197">Examples include choosing a stronger cipher and redirecting all traffic over HTTP to HTTPS.</span></span>

* <span data-ttu-id="ef5d1-198">Agregar un encabezado `HTTP Strict-Transport-Security` (HSTS) garantiza que todas las solicitudes siguientes realizadas por el cliente sean solo a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-198">Adding an `HTTP Strict-Transport-Security` (HSTS) header ensures all subsequent requests made by the client are over HTTPS only.</span></span>

* <span data-ttu-id="ef5d1-199">No agregue el encabezado Strict-Transport-Security o elija un valor de `max-age` adecuado si tiene previsto deshabilitar SSL en el futuro.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-199">Do not add the Strict-Transport-Security header or chose an appropriate `max-age` if you plan to disable SSL in the future.</span></span>

<span data-ttu-id="ef5d1-200">Agregue el archivo de configuración */etc/nginx/proxy.conf*:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-200">Add the */etc/nginx/proxy.conf* configuration file:</span></span>

[!code-nginx[Main](linuxproduction/proxy.conf)]

<span data-ttu-id="ef5d1-201">Edite el archivo de configuración */etc/nginx/nginx.conf*.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-201">Edit the */etc/nginx/nginx.conf* configuration file.</span></span> <span data-ttu-id="ef5d1-202">El ejemplo contiene las dos secciones `http` y `server` en un archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-202">The example contains both `http` and `server` sections in one configuration file.</span></span>

[!code-nginx[Main](../publishing/linuxproduction/nginx.conf?highlight=2)]

#### <a name="secure-nginx-from-clickjacking"></a><span data-ttu-id="ef5d1-203">Proteger Nginx frente al secuestro de clic</span><span class="sxs-lookup"><span data-stu-id="ef5d1-203">Secure Nginx from clickjacking</span></span>
<span data-ttu-id="ef5d1-204">El secuestro de clic es una técnica malintencionada para recopilar los clics de un usuario infectado.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-204">Clickjacking is a malicious technique to collect an infected user's clicks.</span></span> <span data-ttu-id="ef5d1-205">El secuestro de clic engaña a la víctima (visitante) para que haga clic en un sitio infectado.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-205">Clickjacking tricks the victim (visitor) into clicking on an infected site.</span></span> <span data-ttu-id="ef5d1-206">Use X-FRAME-OPTIONS para proteger su sitio.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-206">Use X-FRAME-OPTIONS to secure your site.</span></span>

<span data-ttu-id="ef5d1-207">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-207">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="ef5d1-208">Agregue la línea `add_header X-Frame-Options "SAMEORIGIN";` y guarde el archivo, después, reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-208">Add the line `add_header X-Frame-Options "SAMEORIGIN";` and save the file, then restart Nginx.</span></span>

#### <a name="mime-type-sniffing"></a><span data-ttu-id="ef5d1-209">Examen de tipo MIME</span><span class="sxs-lookup"><span data-stu-id="ef5d1-209">MIME-type sniffing</span></span>

<span data-ttu-id="ef5d1-210">Este encabezado evita que la mayoría de los exploradores examinen el MIME de una respuesta fuera del tipo de contenido declarado, ya que el encabezado indica al explorador que no reemplace el tipo de contenido de respuesta.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-210">This header prevents most browsers from MIME-sniffing a response away from the declared content type, as the header instructs the browser not to override the response content type.</span></span> <span data-ttu-id="ef5d1-211">Con la opción `nosniff`, si el servidor indica que el contenido es "text/html", el explorador lo representa como "text/html".</span><span class="sxs-lookup"><span data-stu-id="ef5d1-211">With the `nosniff` option, if the server says the content is "text/html", the browser renders it as "text/html".</span></span>

<span data-ttu-id="ef5d1-212">Edite el archivo *nginx.conf*:</span><span class="sxs-lookup"><span data-stu-id="ef5d1-212">Edit the *nginx.conf* file:</span></span>

```bash
sudo nano /etc/nginx/nginx.conf
```

<span data-ttu-id="ef5d1-213">Agregue la línea `add_header X-Content-Type-Options "nosniff";`, guarde el archivo y después reinicie Nginx.</span><span class="sxs-lookup"><span data-stu-id="ef5d1-213">Add the line `add_header X-Content-Type-Options "nosniff";` and save the file, then restart Nginx.</span></span>
---
uid: single-page-application/overview/templates/emberjs-template
title: Plantilla de EmberJS | Documentos de Microsoft
author: xqiu
description: Plantilla de EmberJS
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="emberjs-template"></a><span data-ttu-id="b68f4-103">Plantilla de EmberJS</span><span class="sxs-lookup"><span data-stu-id="b68f4-103">EmberJS template</span></span>
====================
<span data-ttu-id="b68f4-104">por [Xinyang Qiu](https://github.com/xqiu)</span><span class="sxs-lookup"><span data-stu-id="b68f4-104">by [Xinyang Qiu](https://github.com/xqiu)</span></span>

> <span data-ttu-id="b68f4-105">La plantilla de MVC EmberJS escribe Nathan Totten, Thiago Santos y Xinyang Qiu.</span><span class="sxs-lookup"><span data-stu-id="b68f4-105">The EmberJS MVC Template is written by Nathan Totten, Thiago Santos, and Xinyang Qiu.</span></span>
> 
> [<span data-ttu-id="b68f4-106">Descargue la plantilla de MVC EmberJS</span><span class="sxs-lookup"><span data-stu-id="b68f4-106">Download the EmberJS MVC Template</span></span>](https://go.microsoft.com/fwlink/?LinkId=282647)


<span data-ttu-id="b68f4-107">La plantilla de SPA EmberJS está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web de cliente interactivas mediante EmberJS.</span><span class="sxs-lookup"><span data-stu-id="b68f4-107">The EmberJS SPA template is designed to get you started quickly building interactive client-side web apps using EmberJS.</span></span>

<span data-ttu-id="b68f4-108">"Aplicación de página" (contraseña segura SPA) es el término general para una aplicación web que se carga una página HTML única y, a continuación, actualiza la página de forma dinámica, en lugar de cargar páginas nuevas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-108">"Single-page application" (SPA) is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="b68f4-109">Después de la carga de la página inicial, la aplicación SPA se comunica con el servidor a través de solicitudes de AJAX.</span><span class="sxs-lookup"><span data-stu-id="b68f4-109">After the initial page load, the SPA talks with the server through AJAX requests.</span></span>

![](emberjs-template/_static/image1.png)

<span data-ttu-id="b68f4-110">AJAX no es nada nuevo, pero en la actualidad existen marcos de JavaScript que resulten más fácil generar y mantener una gran aplicación SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="b68f4-110">AJAX is nothing new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="b68f4-111">Además, HTML5 y CSS3 se conseguir que sea más fácil de crear interfaces de usuario enriquecidas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-111">Also, HTML 5 and CSS3 are making it easier to create rich UIs.</span></span>

<span data-ttu-id="b68f4-112">La plantilla de SPA EmberJS utiliza el [Ember](http://emberjs.com/) biblioteca de JavaScript para controlar las actualizaciones de la página de solicitudes de AJAX.</span><span class="sxs-lookup"><span data-stu-id="b68f4-112">The EmberJS SPA Template uses the [Ember](http://emberjs.com/) JavaScript library to handle page updates from AJAX requests.</span></span> <span data-ttu-id="b68f4-113">Ember.js usa el enlace de datos para sincronizar la página con los datos más recientes.</span><span class="sxs-lookup"><span data-stu-id="b68f4-113">Ember.js uses data binding to synchronize the page with the latest data.</span></span> <span data-ttu-id="b68f4-114">De este modo, no tienes que escribir el código que le guía a través de los datos JSON y actualiza el DOM.</span><span class="sxs-lookup"><span data-stu-id="b68f4-114">That way, you don't have to write any of the code that walks through the JSON data and updates the DOM.</span></span> <span data-ttu-id="b68f4-115">En su lugar, coloque atributos declarativos en el código HTML que indican Ember.js cómo presentar los datos.</span><span class="sxs-lookup"><span data-stu-id="b68f4-115">Instead, you put declarative attributes in the HTML that tell Ember.js how to present the data.</span></span>

<span data-ttu-id="b68f4-116">En el servidor, es casi idéntica a la plantilla de EmberJS la [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md).</span><span class="sxs-lookup"><span data-stu-id="b68f4-116">On the server side, the EmberJS template is almost identical to the [KnockoutJS SPA template](../introduction/knockoutjs-template.md).</span></span> <span data-ttu-id="b68f4-117">Usa ASP.NET MVC para servir documentos HTML y ASP.NET Web API para controlar las solicitudes de AJAX del cliente.</span><span class="sxs-lookup"><span data-stu-id="b68f4-117">It uses ASP.NET MVC to serve HTML documents, and ASP.NET Web API to handle AJAX requests from the client.</span></span> <span data-ttu-id="b68f4-118">Para obtener más información sobre los aspectos de la plantilla, consulte el [KnockoutJS plantilla](../introduction/knockoutjs-template.md) documentación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-118">For more information about those aspects of the template, refer to the [KnockoutJS template](../introduction/knockoutjs-template.md) documentation.</span></span> <span data-ttu-id="b68f4-119">En este tema se centra en las diferencias entre la plantilla de Knockout y la plantilla EmberJS.</span><span class="sxs-lookup"><span data-stu-id="b68f4-119">This topic focuses on the differences between the Knockout template and the EmberJS template.</span></span>

## <a name="create-an-emberjs-spa-template-project"></a><span data-ttu-id="b68f4-120">Crear un proyecto de plantilla SPA EmberJS</span><span class="sxs-lookup"><span data-stu-id="b68f4-120">Create an EmberJS SPA Template Project</span></span>

<span data-ttu-id="b68f4-121">Descargue e instale la plantilla, haga clic en el botón de descarga más arriba.</span><span class="sxs-lookup"><span data-stu-id="b68f4-121">Download and install the template by clicking the Download button above.</span></span> <span data-ttu-id="b68f4-122">Debe reiniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="b68f4-122">You might need to restart Visual Studio.</span></span>

<span data-ttu-id="b68f4-123">En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo.</span><span class="sxs-lookup"><span data-stu-id="b68f4-123">In the **Templates** pane, select **Installed Templates** and expand the **Visual C#** node.</span></span> <span data-ttu-id="b68f4-124">En **Visual C#**, seleccione **Web**.</span><span class="sxs-lookup"><span data-stu-id="b68f4-124">Under **Visual C#**, select **Web**.</span></span> <span data-ttu-id="b68f4-125">En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**.</span><span class="sxs-lookup"><span data-stu-id="b68f4-125">In the list of project templates, select **ASP.NET MVC 4 Web Application**.</span></span> <span data-ttu-id="b68f4-126">Denomine el proyecto y haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="b68f4-126">Name the project and click **OK**.</span></span>

![](emberjs-template/_static/image2.png)

<span data-ttu-id="b68f4-127">En el **nuevo proyecto** asistente, seleccione **Ember.js SPA proyecto**.</span><span class="sxs-lookup"><span data-stu-id="b68f4-127">In the **New Project** wizard, select **Ember.js SPA Project**.</span></span>

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a><span data-ttu-id="b68f4-128">Introducción a la plantilla EmberJS SPA</span><span class="sxs-lookup"><span data-stu-id="b68f4-128">EmberJS SPA Template Overview</span></span>

<span data-ttu-id="b68f4-129">La plantilla de EmberJS utiliza una combinación de jQuery, Ember.js, Handlebars.js para crear una interfaz de usuario interactivo, suave.</span><span class="sxs-lookup"><span data-stu-id="b68f4-129">The EmberJS template uses a combination of jQuery, Ember.js, Handlebars.js to create a smooth, interactive UI.</span></span>

<span data-ttu-id="b68f4-130">Ember.js es una biblioteca de JavaScript que use un patrón MVC del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="b68f4-130">Ember.js is a JavaScript library that uses a client-side MVC pattern.</span></span>

- <span data-ttu-id="b68f4-131">A *plantilla*escrito en el lenguaje de definición de plantillas manillares, describe la interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-131">A *template*, written in the Handlebars templating language, describes the application user interface.</span></span> <span data-ttu-id="b68f4-132">En modo de lanzamiento, el [compilador manillares](https://github.com/Myslik/csharp-ember-handlebars) se utiliza para agrupar y compilar la plantilla manillares.</span><span class="sxs-lookup"><span data-stu-id="b68f4-132">In release mode, the [Handlebars compiler](https://github.com/Myslik/csharp-ember-handlebars) is used to bundle and compile the handlebars template.</span></span>
- <span data-ttu-id="b68f4-133">A *modelo* almacena los datos de aplicación que obtiene del servidor (listas de tareas y elementos de lista de tareas).</span><span class="sxs-lookup"><span data-stu-id="b68f4-133">A *model* stores the application data that it gets from the server (ToDo lists and ToDo items).</span></span>
- <span data-ttu-id="b68f4-134">A *controlador* almacena el estado de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-134">A *controller* stores application state.</span></span> <span data-ttu-id="b68f4-135">A menudo, controladores de presentan datos de modelo a las plantillas correspondientes.</span><span class="sxs-lookup"><span data-stu-id="b68f4-135">Controllers often present model data to the corresponding templates.</span></span>
- <span data-ttu-id="b68f4-136">A *vista* traduce primitivos eventos de la aplicación y los pasa al controlador.</span><span class="sxs-lookup"><span data-stu-id="b68f4-136">A *view* translates primitive events from the application and passes these to the controller.</span></span>
- <span data-ttu-id="b68f4-137">A *enrutador* administra el estado de aplicación, manteniendo las direcciones URL y las plantillas sincronizadas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-137">A *router* manages application state, keeping URLs and templates in sync.</span></span>

<span data-ttu-id="b68f4-138">Además, la biblioteca de datos de Ember puede utilizarse para sincronizar objetos JSON (obtenidos del servidor a través de la API de REST) y los modelos de cliente.</span><span class="sxs-lookup"><span data-stu-id="b68f4-138">In addition, the Ember Data library can be used to synchronize JSON objects (obtained from the server through a RESTful API) and the client models.</span></span>

<span data-ttu-id="b68f4-139">La plantilla EmberJS SPA organiza las secuencias de comandos en ocho capas:</span><span class="sxs-lookup"><span data-stu-id="b68f4-139">The EmberJS SPA template organizes the scripts into eight layers:</span></span>

- <span data-ttu-id="b68f4-140">webapi\_adapter.js, webapi\_serializer.js: extiende la biblioteca de datos de Ember para que funcione con ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="b68f4-140">webapi\_adapter.js, webapi\_serializer.js: Extends the Ember Data library to work with ASP.NET Web API.</span></span>
- <span data-ttu-id="b68f4-141">Scripts/helpers.js: Define las aplicaciones auxiliares Ember manillares nueva.</span><span class="sxs-lookup"><span data-stu-id="b68f4-141">Scripts/helpers.js: Defines new Ember Handlebars helpers.</span></span>
- <span data-ttu-id="b68f4-142">Scripts/App.js: La aplicación se crea y configura el adaptador y el serializador.</span><span class="sxs-lookup"><span data-stu-id="b68f4-142">Scripts/app.js: Creates the app and configures the adapter and serializer.</span></span>
- <span data-ttu-id="b68f4-143">Las secuencias decomandos/aplicación/modelos/\*.js: define los modelos.</span><span class="sxs-lookup"><span data-stu-id="b68f4-143">Scripts/app/models/\*.js: Defines the models.</span></span>
- <span data-ttu-id="b68f4-144">Las secuencias decomandos/aplicación/vistas/\*.js: define las vistas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-144">Scripts/app/views/\*.js: Defines the views.</span></span>
- <span data-ttu-id="b68f4-145">Las secuencias decomandos/aplicación/controladores/\*.js: define los controladores.</span><span class="sxs-lookup"><span data-stu-id="b68f4-145">Scripts/app/controllers/\*.js: Defines the controllers.</span></span>
- <span data-ttu-id="b68f4-146">Las secuencias de comandos/aplicación/rutas, Scripts/app/router.js: Define las rutas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-146">Scripts/app/routes, Scripts/app/router.js: Defines the routes.</span></span>
- <span data-ttu-id="b68f4-147">Plantillas /\*.hbs: define las plantillas manillares.</span><span class="sxs-lookup"><span data-stu-id="b68f4-147">Templates/\*.hbs: Defines the handlebars templates.</span></span>

<span data-ttu-id="b68f4-148">Echemos un vistazo a algunas de estas secuencias de comandos con más detalle.</span><span class="sxs-lookup"><span data-stu-id="b68f4-148">Let's look at some of these scripts in more detail.</span></span>

## <a name="models"></a><span data-ttu-id="b68f4-149">Modelos</span><span class="sxs-lookup"><span data-stu-id="b68f4-149">Models</span></span>

<span data-ttu-id="b68f4-150">Los modelos se definen en la carpeta Scripts/aplicación/models.</span><span class="sxs-lookup"><span data-stu-id="b68f4-150">The models are defined in the Scripts/app/models folder.</span></span> <span data-ttu-id="b68f4-151">Hay dos archivos de modelo: todoItem.js y todoList.js.</span><span class="sxs-lookup"><span data-stu-id="b68f4-151">There are two model files: todoItem.js and todoList.js.</span></span>

<span data-ttu-id="b68f4-152">**todo.Model.js** define los modelos de cliente (explorador) para las listas de tareas pendientes.</span><span class="sxs-lookup"><span data-stu-id="b68f4-152">**todo.model.js** defines the client-side (browser) models for the to-do lists.</span></span> <span data-ttu-id="b68f4-153">Hay dos clases de modelo: todoItem y todoList.</span><span class="sxs-lookup"><span data-stu-id="b68f4-153">There are two model classes: todoItem and todoList.</span></span> <span data-ttu-id="b68f4-154">En Ember, los modelos son subclases de DS. Modelo.</span><span class="sxs-lookup"><span data-stu-id="b68f4-154">In Ember, models are subclasses of DS.Model.</span></span> <span data-ttu-id="b68f4-155">Un modelo puede tener propiedades con atributos:</span><span class="sxs-lookup"><span data-stu-id="b68f4-155">A model can have properties with attributes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

<span data-ttu-id="b68f4-156">Los modelos pueden definir relaciones con otros modelos:</span><span class="sxs-lookup"><span data-stu-id="b68f4-156">Models can define relationships to other models:</span></span>

[!code-css[Main](emberjs-template/samples/sample2.css)]

<span data-ttu-id="b68f4-157">Los modelos pueden calcular propiedades que se enlazan a otras propiedades:</span><span class="sxs-lookup"><span data-stu-id="b68f4-157">Models can have computed properties that bind to other properties:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

<span data-ttu-id="b68f4-158">Los modelos pueden tener funciones de observador, que se invocan cuando cambia una propiedad observada:</span><span class="sxs-lookup"><span data-stu-id="b68f4-158">Models can have observer functions, which are invoked when an observed property changes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a><span data-ttu-id="b68f4-159">Vistas</span><span class="sxs-lookup"><span data-stu-id="b68f4-159">Views</span></span>

<span data-ttu-id="b68f4-160">Las vistas se definen en la carpeta Scripts/aplicación/vistas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-160">The views are defined in the Scripts/app/views folder.</span></span> <span data-ttu-id="b68f4-161">Una vista convierte los eventos del interfaz de usuario de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-161">A view translates events from the application UI.</span></span> <span data-ttu-id="b68f4-162">Un controlador de eventos puede devolver la llamada a funciones de controlador o simplemente llamar directamente al contexto de datos.</span><span class="sxs-lookup"><span data-stu-id="b68f4-162">An event handler can call back to controller functions, or simply call the data context directly.</span></span>

<span data-ttu-id="b68f4-163">Por ejemplo, el código siguiente es de views/TodoItemEditView.js.</span><span class="sxs-lookup"><span data-stu-id="b68f4-163">For example, the following code is from views/TodoItemEditView.js.</span></span> <span data-ttu-id="b68f4-164">Define el control de eventos para un campo de texto de entrada.</span><span class="sxs-lookup"><span data-stu-id="b68f4-164">It defines the event handling for an input text field.</span></span>

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a><span data-ttu-id="b68f4-165">Controlador</span><span class="sxs-lookup"><span data-stu-id="b68f4-165">Controller</span></span>

<span data-ttu-id="b68f4-166">Los controladores se definen en la carpeta de secuencias de comandos/aplicación/controladores.</span><span class="sxs-lookup"><span data-stu-id="b68f4-166">The controllers are defined in the Scripts/app/controllers folder.</span></span> <span data-ttu-id="b68f4-167">Para representar un único modelo, extender `Ember.ObjectController`:</span><span class="sxs-lookup"><span data-stu-id="b68f4-167">To represent a single model, extend `Ember.ObjectController`:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

<span data-ttu-id="b68f4-168">Un controlador también puede representar una colección de modelos mediante la extensión `Ember.ArrayController`.</span><span class="sxs-lookup"><span data-stu-id="b68f4-168">A controller can also represent a collection of models by extending `Ember.ArrayController`.</span></span> <span data-ttu-id="b68f4-169">Por ejemplo, el TodoListController representa una matriz de `todoList` objetos.</span><span class="sxs-lookup"><span data-stu-id="b68f4-169">For example, the TodoListController represents an array of `todoList` objects.</span></span> <span data-ttu-id="b68f4-170">El controlador se ordena por identificador de todoList, en orden descendente:</span><span class="sxs-lookup"><span data-stu-id="b68f4-170">The controller sorts by todoList ID, in descending order:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

<span data-ttu-id="b68f4-171">El controlador define una función denominada `addTodoList`, que crea un nuevo todoList y lo agrega a la matriz.</span><span class="sxs-lookup"><span data-stu-id="b68f4-171">The controller defines a function named `addTodoList`, which creates a new todoList and adds it to the array.</span></span> <span data-ttu-id="b68f4-172">Para ver cómo llama a esta función, abra el archivo de plantilla denominado todoListTemplate.html, en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-172">To see how this function gets called, open the template file named todoListTemplate.html, in the Templates folder.</span></span> <span data-ttu-id="b68f4-173">El siguiente código de plantilla enlaza un botón a la `addTodoList` función:</span><span class="sxs-lookup"><span data-stu-id="b68f4-173">The following template code binds a button to the `addTodoList` function:</span></span>

[!code-html[Main](emberjs-template/samples/sample8.html)]

<span data-ttu-id="b68f4-174">El controlador también contiene un `error` propiedad, que contiene un mensaje de error.</span><span class="sxs-lookup"><span data-stu-id="b68f4-174">The controller also contains an `error` property, which holds an error message.</span></span> <span data-ttu-id="b68f4-175">Este es el código de plantilla para mostrar el mensaje de error (también en todoListTemplate.html):</span><span class="sxs-lookup"><span data-stu-id="b68f4-175">Here is the template code to display the error message (also in todoListTemplate.html):</span></span>

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a><span data-ttu-id="b68f4-176">Rutas</span><span class="sxs-lookup"><span data-stu-id="b68f4-176">Routes</span></span>

<span data-ttu-id="b68f4-177">Router.js define las rutas y la plantilla predeterminada para mostrar, Establece el estado de aplicación y coincide con las direcciones URL a las rutas:</span><span class="sxs-lookup"><span data-stu-id="b68f4-177">Router.js defines the routes and the default template to display, sets up application state, and matches URLs to routes:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

<span data-ttu-id="b68f4-178">TodoListRoute.js carga datos para el TodoListRoute reemplazando la función setupController:</span><span class="sxs-lookup"><span data-stu-id="b68f4-178">TodoListRoute.js loads data for the TodoListRoute by overriding the setupController function:</span></span>

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

<span data-ttu-id="b68f4-179">Ember usa las convenciones de nomenclatura para que coincida con las direcciones URL, los nombres de ruta, controladores y plantillas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-179">Ember uses naming conventions to match URLs, route names, controllers, and templates.</span></span> <span data-ttu-id="b68f4-180">Para obtener más información, consulte [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) en la documentación de EmberJS.</span><span class="sxs-lookup"><span data-stu-id="b68f4-180">For more information, see [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) at the EmberJS documentation.</span></span>

## <a name="templates"></a><span data-ttu-id="b68f4-181">Plantillas</span><span class="sxs-lookup"><span data-stu-id="b68f4-181">Templates</span></span>

<span data-ttu-id="b68f4-182">La carpeta de plantillas contiene cuatro plantillas:</span><span class="sxs-lookup"><span data-stu-id="b68f4-182">The Templates folder contains four templates:</span></span>

- <span data-ttu-id="b68f4-183">Application.HBS: la plantilla predeterminada que se representa cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-183">application.hbs: The default template that is rendered when the application is started.</span></span>
- <span data-ttu-id="b68f4-184">About.HBS: la plantilla de la ruta "/ punto".</span><span class="sxs-lookup"><span data-stu-id="b68f4-184">about.hbs: The template for the "/about" route.</span></span>
- <span data-ttu-id="b68f4-185">index.HBS: la plantilla para la raíz de la ruta "/".</span><span class="sxs-lookup"><span data-stu-id="b68f4-185">index.hbs: The template for the root "/" route.</span></span>
- <span data-ttu-id="b68f4-186">todoList.hbs: la plantilla para la "/ todo" ruta.</span><span class="sxs-lookup"><span data-stu-id="b68f4-186">todoList.hbs: The template for the "/todo" route.</span></span>
- <span data-ttu-id="b68f4-187">\_NavBar.HBS: la plantilla define el menú de navegación.</span><span class="sxs-lookup"><span data-stu-id="b68f4-187">\_navbar.hbs: The template defines the navigation menu.</span></span>

<span data-ttu-id="b68f4-188">La plantilla de aplicación actúa como una página maestra.</span><span class="sxs-lookup"><span data-stu-id="b68f4-188">The application template acts like a master page.</span></span> <span data-ttu-id="b68f4-189">Contiene un encabezado y un pie de página, una "{{toma}}" para insertar otras plantillas de función de la ruta.</span><span class="sxs-lookup"><span data-stu-id="b68f4-189">It contains a header, a footer, and an "{{outlet}}" to insert other templates in depending on the route.</span></span> <span data-ttu-id="b68f4-190">Para obtener más información acerca de las plantillas de aplicación en Ember, consulte [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span><span class="sxs-lookup"><span data-stu-id="b68f4-190">For more information about application templates in Ember, see [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).</span></span>

<span data-ttu-id="b68f4-191">El "/ todoList" plantilla contiene dos expresiones de bucle.</span><span class="sxs-lookup"><span data-stu-id="b68f4-191">The "/todoList" template contains two loop expressions.</span></span> <span data-ttu-id="b68f4-192">El bucle exterior es `{{#each controller}}`y el interior bucle es `{{#each todos}}`.</span><span class="sxs-lookup"><span data-stu-id="b68f4-192">The outside loop is `{{#each controller}}`, and the inside loop is `{{#each todos}}`.</span></span> <span data-ttu-id="b68f4-193">El código siguiente muestra un integrada `Ember.Checkbox` ver una personalizada `App.TodoItemEditView`y un vínculo con un `deleteTodo` acción.</span><span class="sxs-lookup"><span data-stu-id="b68f4-193">The following code shows a built-in `Ember.Checkbox` view, a customized `App.TodoItemEditView`, and a link with a `deleteTodo` action.</span></span>

[!code-html[Main](emberjs-template/samples/sample12.html)]

<span data-ttu-id="b68f4-194">El `HtmlHelperExtensions` (clase), definida en Controllers/HtmlHelperExensions.cs, define una aplicación auxiliar de función para almacenar en memoria caché e Insertar plantilla archivos al **depurar** está establecido en **true** en el archivo Web.config.</span><span class="sxs-lookup"><span data-stu-id="b68f4-194">The `HtmlHelperExtensions` class, defined in Controllers/HtmlHelperExensions.cs, defines a helper function to cache and insert template files when **debug** is set to **true** in the Web.config file.</span></span> <span data-ttu-id="b68f4-195">Esta función se llama desde el archivo de vista de MVC de ASP.NET definido en Views/Home/App.cshtml:</span><span class="sxs-lookup"><span data-stu-id="b68f4-195">This function is called from the ASP.NET MVC view file defined in Views/Home/App.cshtml:</span></span>

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

<span data-ttu-id="b68f4-196">Se llama sin argumentos, la función representa todos los archivos de plantilla en la carpeta de plantillas.</span><span class="sxs-lookup"><span data-stu-id="b68f4-196">Called with no arguments, the function renders all of the template files in the Templates folder.</span></span> <span data-ttu-id="b68f4-197">También puede especificar una subcarpeta o un archivo de plantilla específica.</span><span class="sxs-lookup"><span data-stu-id="b68f4-197">You can also specify a subfolder or a specific template file.</span></span>

<span data-ttu-id="b68f4-198">Cuando **depurar** es **false** en el archivo Web.config, la aplicación incluye el elemento de agrupación "~/bundles/templates".</span><span class="sxs-lookup"><span data-stu-id="b68f4-198">When **debug** is **false** in Web.config, the application includes the bundle item "~/bundles/templates".</span></span> <span data-ttu-id="b68f4-199">Este elemento de agrupación se agrega en BundleConfig.cs, mediante la biblioteca de compilador manillares:</span><span class="sxs-lookup"><span data-stu-id="b68f4-199">This bundle item is added in BundleConfig.cs, using the Handlebars compiler library:</span></span>

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
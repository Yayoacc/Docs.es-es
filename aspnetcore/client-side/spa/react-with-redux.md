---
title: Uso de la plantilla de proyecto React-with-Redux con ASP.NET Core
author: SteveSandersonMS
description: Aprenda cómo comenzar a trabajar con la plantilla de proyecto de aplicación de página única (SPA) de ASP.NET Core para React with Redux y create-react-app.
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/react-with-redux
ms.openlocfilehash: dab3d20865250aae548bff4614e631dd7c73b46f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291488"
---
# <a name="use-the-react-with-redux-project-template-with-aspnet-core"></a><span data-ttu-id="beb15-103">Uso de la plantilla de proyecto React-with-Redux con ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="beb15-103">Use the React-with-Redux project template with ASP.NET Core</span></span>

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> <span data-ttu-id="beb15-104">Esta documentación no trata sobre la plantilla de proyecto de React-with-Redux incluida en ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="beb15-104">This documentation isn't about the React-with-Redux project template included in ASP.NET Core 2.0.</span></span> <span data-ttu-id="beb15-105">Trata sobre la nueva plantilla de React-with-Redux que puede actualizar manualmente.</span><span class="sxs-lookup"><span data-stu-id="beb15-105">It's about the newer React-with-Redux template to which you can update manually.</span></span> <span data-ttu-id="beb15-106">La plantilla se incluye de forma predeterminada en ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="beb15-106">The template is included in ASP.NET Core 2.1 by default.</span></span>

::: moniker-end

<span data-ttu-id="beb15-107">La plantilla de proyecto actualizada de React-with-Redux ofrece un práctico punto de partida para las aplicaciones ASP.NET Core que usan React, Redux y las convenciones [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) para implementar una completa interfaz de usuario (UI) en el lado cliente.</span><span class="sxs-lookup"><span data-stu-id="beb15-107">The updated React-with-Redux project template provides a convenient starting point for ASP.NET Core apps using React, Redux, and [create-react-app](https://github.com/facebookincubator/create-react-app) (CRA) conventions to implement a rich, client-side user interface (UI).</span></span>

<span data-ttu-id="beb15-108">A excepción del comando de creación del proyecto, toda la información sobre la plantilla de React-with-Redux es igual que la de la plantilla de React.</span><span class="sxs-lookup"><span data-stu-id="beb15-108">With the exception of the project creation command, all information about the React-with-Redux template is the same as the React template.</span></span> <span data-ttu-id="beb15-109">Para crear este tipo de proyecto, ejecute `dotnet new reactredux` en lugar de `dotnet new react`.</span><span class="sxs-lookup"><span data-stu-id="beb15-109">To create this project type, run `dotnet new reactredux` instead of `dotnet new react`.</span></span> <span data-ttu-id="beb15-110">Para más información sobre la funcionalidad común a ambas plantillas basadas en React, consulte la [documentación de la plantilla de React](xref:spa/react).</span><span class="sxs-lookup"><span data-stu-id="beb15-110">For more information about the functionality common to both React-based templates, see [React template documentation](xref:spa/react).</span></span>
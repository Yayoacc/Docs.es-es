---
title: "Vistas y métodos de controlador"
author: rick-anderson
description: "Trabajar con los métodos de controlador, vistas y DataAnnotations"
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: c7313211-bb71-4adf-babe-8e72603cc0ce
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app-xplat/controller-methods-views
ms.openlocfilehash: 87516295c6e8c49b492312afda80c92c968e778b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="controller-methods-and-views"></a>Vistas y métodos de controlador

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

La aplicación de películas pinta bien, pero la presentación no es ideal. No queremos ver la hora (12:00:00 a.m. en la imagen siguiente) y **ReleaseDate** deben ser dos palabras.

![Vista de índice: Release Date es una palabra (sin espacios) y en todas las fechas de lanzamiento de películas se muestra una hora de 12 AM](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

Abra el archivo *Models/Movie.cs* y agregue las líneas resaltadas que se muestran a continuación:

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieDate.cs?name=snippet_1&highlight=2,11-12)]

Compile y ejecute la aplicación.

<!-- include start
![MVC Movie application open browser showing movie data](../../tutorials/first-mvc-app/working-with-sql/_static/m55.png)

 -->

[!INCLUDE[adding-model](../../includes/mvc-intro/controller-methods-views.md)]

>[!div class="step-by-step"]
[Anterior: Trabajar con SQLite](working-with-sql.md)
[Siguiente: Agregar búsqueda](search.md)  
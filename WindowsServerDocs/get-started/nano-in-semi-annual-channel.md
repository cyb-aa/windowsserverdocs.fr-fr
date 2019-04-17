---
title: Modifications apportées à NanoServer dans le canal semi-annuel de WindowsServer
description: Dans le nouveau modèle de maintenance de Windows Server, Nano Server est un système d’exploitation de conteneur uniquement, dont certaines fonctionnalités sont modifiées.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jaimeo
ms.localizationpriority: medium
ms.date: 05/02/2018
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: 7e68d292c32ce58c786a3242203330fcae985913
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/09/2018
ms.locfileid: "2711954"
---
# Modifications apportées à NanoServer dans le canal semi-annuel de WindowsServer

>S’applique à: WindowsServer (canal semi-annuel)


Comme expliqué dans la rubrique [Présentation du canal semi-annuel de WindowsServer](semi-annual-channel-overview.md), Windows Server, version1803 est la version la plus récente du canal semi-annuel.

Si vous utilisez déjà Nano Server, ce modèle de maintenance sera familier, puisqu'il est déjà traité par le modèle CBB (Current Branch for Business). Le nouveau canal semi-annuel de WindowsServer est simplement le nouveau nom de ce modèle. Dans ce modèle, les publications des mises à jour des fonctionnalités de Nano Server sont prévues deux à trois fois par an.

Toutefois, avec cette version de Windows Server, version1803, Nano Server n'est disponible que sous forme d'une **image du système d’exploitation de base du conteneur**. Vous devez l’exécuter en tant que conteneur dans un hôte de conteneur, par exemple, une installation minimale de WindowsServer. L'exécution d'un conteneur basé sur Nano Server dans cette version diffère des versions antérieures sur les points suivants:

- Nano Server a été optimisé pour les applications .NET Core.
- Nano Server est encore plus petit que la version de Windows Server2016.
- PowerShellCore, .NETCore et WMI ne sont plus inclus par défaut, mais vous pouvez inclure des packages de conteneur [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) et [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) lors de la création de votre conteneur.
- Nano Server n'inclut plus de pile de traitements. Microsoft publie un conteneur Nano mis à jour sur Docker Hub que vous redéployez.
- La résolution des problèmes du nouveau conteneur Nano s'effectue à l’aide de Docker.
- Vous pouvez maintenant exécuter des conteneurs Nano sur IoT Standard.

## Rubriques associées
Au début du programme Insider, vous trouverez plus d’informations dans la [Documentation sur les conteneurs Windows](http://aka.ms/windowscontainers).

---
title: Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server
description: Dans le nouveau modèle de service Windows Server, Nano Server est un système d’exploitation de conteneur uniquement, dont certaines fonctionnalités sont modifiées.
ms.prod: Windows Server
ms.mktglfcycl: manage
ms.sitesec: library
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.date: 05/21/2019
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: a270334d-42a7-46ff-8eed-d8656a276544
ms.openlocfilehash: c12ca84826a92fa045eb84b55e7406392161280b
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66452805"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server

>S'applique à : Windows Server, canal semi-annuel

Si vous exécutez déjà Nano Server, le modèle de service [Canal semi-annuel Windows Server](../get-started-19/servicing-channels-19.md) vous sera familier, puisqu’il est déjà traité par le modèle CBB (Current Branch for Business). Canal semi-annuel Windows Server est simplement le nouveau nom de ce modèle. Dans ce modèle, les publications des mises à jour des fonctionnalités de Nano Server sont prévues deux à trois fois par an.

Toutefois, à compter de Windows Server, version 1803, Nano Server est uniquement disponible sous forme d’**image de système d’exploitation de base du conteneur**. Vous devez l’exécuter en tant que conteneur dans un hôte de conteneur, par exemple une installation minimale de Windows Server. L’exécution d’un conteneur basé sur Nano Server dans cette version diffère des versions antérieures sur les points suivants :

- Nano Server a été optimisé pour les applications .NET Core.
- Nano Server est encore plus petit que la version Windows Server 2016.
- PowerShell Core, .NET Core et WMI ne sont plus inclus par défaut, mais vous pouvez inclure des packages de conteneur [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) et [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) lors de la création de votre conteneur.
- Nano Server n’inclut plus de pile de maintenance. Microsoft publie un conteneur Nano mis à jour sur Docker Hub que vous redéployez.
- La résolution des problèmes du nouveau conteneur Nano s’effectue à l’aide de Docker.
- Vous pouvez maintenant exécuter des conteneurs Nano sur IoT Core.

## <a name="related-topics"></a>Rubriques connexes

- [Documentation sur les conteneurs Windows](http://aka.ms/windowscontainers)
- [Présentation du canal semi-annuel de Windows Server](../get-started-19/servicing-channels-19.md)

---
title: Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server
description: Dans le nouveau modèle de maintenance de Windows Server, Nano Server est un système d’exploitation de conteneur uniquement, dont certaines fonctionnalités sont modifiées.
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
ms.openlocfilehash: c9fede02b90e285803a8bcdbc983f264d65a4589
ms.sourcegitcommit: c8cc0b25ba336a2aafaabc92b19fe8faa56be32b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65976515"
---
# <a name="changes-to-nano-server-in-windows-server-semi-annual-channel"></a>Modifications apportées à Nano Server dans le canal semi-annuel de Windows Server

>S'applique à : Windows Server, canal semi-annuel

Si vous exécutez déjà Nano Server, le [canal semi-annuel de serveur fenêtre](..\get-started-19\servicing-channels-19.md) modèle de service sera familier, dans la mesure où il a été précédemment traitée par la branche actuelle pour le modèle d’entreprise (CBB). Canal semi-annuel de serveur Windows est simplement un nouveau nom pour le même modèle. Dans ce modèle, les publications des mises à jour des fonctionnalités de Nano Server sont prévues deux à trois fois par an.

Toutefois, en commençant par Windows Server, version 1803, Nano Server est disponible uniquement comme un **image de système d’exploitation de base de conteneur**. Vous devez l’exécuter en tant que conteneur dans un hôte de conteneur, par exemple, une installation minimale de Windows Server. L'exécution d'un conteneur basé sur Nano Server dans cette version diffère des versions antérieures sur les points suivants :

- Nano Server a été optimisé pour les applications .NET Core.
- Nano Server est encore plus petit que la version de Windows Server 2016.
- PowerShell Core, .NET Core et WMI ne sont plus inclus par défaut, mais vous pouvez inclure des packages de conteneur [PowerShell Core](https://hub.docker.com/r/microsoft/powershell/) et [.NET Core](https://hub.docker.com/r/microsoft/dotnet/) lors de la création de votre conteneur.
- Nano Server n'inclut plus de pile de traitements. Microsoft publie un conteneur Nano mis à jour sur Docker Hub que vous redéployez.
- La résolution des problèmes du nouveau conteneur Nano s'effectue à l’aide de Docker.
- Vous pouvez maintenant exécuter des conteneurs Nano sur IoT Standard.

## <a name="related-topics"></a>Rubriques connexes

- [Documentation des conteneurs Windows](http://aka.ms/windowscontainers)
- [Vue d’ensemble du canal semi-annuel de serveur fenêtre](..\get-started-19\servicing-channels-19.md)

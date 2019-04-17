---
title: Schéma d’URI de clients Bureau à distance
description: En savoir plus sur le schéma d’identificateur de ressource uniforme pour les clients Bureau à distance
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0c3f1eb6-835c-4522-99ff-56c6ee4bb911
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/11/2018
ms.localizationpriority: medium
ms.openlocfilehash: f2934fed43c8f4feec2f321d684cc3593933eb5d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297468"
---
# Prise en charge du client Bureau à distance schéma d’identificateur URI (Universal Resource)

>S’applique à: Windows Server, version 1803, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

L’activation d’un schéma d’URI Uniform Resource Identifier () donne aux professionnels de l’informatique et aux développeurs un moyen d’intégrer des fonctionnalités des clients Bureau à distance sur différentes plateformes et enrichit l’expérience utilisateur en autorisant: 

- Les applications tierces pour lancer des services Bureau à distance Microsoft et lancer des sessions à distance avec les paramètres prédéfinis (fournis dans le cadre de la chaîne d’URI).
- Utilisateurs finaux à démarrer des connexions à distance à l’aide d’URL préconfigurés.

>[!NOTE]
> À l’aide d’un URI pour se connecter au client Bureau à distance n’est pas pris en charge pour les systèmes d’exploitation Windows, utilisez l’URI avec les appareils Android, iOS et Mac OS.

Services Bureau à distance Microsoft utilise le rdp://query_string de schéma d’URI pour stocker les paramètres d’attribut préconfigurée qui sont utilisées lors du lancement du client. Les chaînes de requête représentent un unique ou un ensemble d’attributs RDP fournis dans l’URL. 

Les attributs RDP sont séparées par le symbole esperluette (&). Par exemple, lors de la connexion à un PC, la chaîne est:

```
rdp://full%20address=s:mypc:3389&audiomode=i:2&disable%20themes=i:1
```

Ce tableau vous donne une liste complète des attributs pris en charge qui peut être utilisée avec les e/s, Mac et Android clients Bureau à distance. (Un «x» dans la colonne de la plateforme indique l’attribut est pris en charge. Les valeurs indiqués par chevrons (<>) représentent les valeurs qui sont pris en charge par les clients Bureau à distance.)

| **Attribut RDP**                                           | **Android** | **Mac** | **iOS** |
|---------------------------------------------------------|---------|-----|-----|
| Autoriser la composition du bureau = i:&lt;0 ou 1&gt;                    | x       | x   | x   |
| Autoriser le lissage des polices = i:<0 ou 1&gt;                         | x       | x   | x   |
| interpréteur de commandes de remplacement = s:&lt;chaîne&gt;                              | x       | x   | x   |
| [audiomode = i:&lt;0, 1 ou 2&gt;](https://technet.microsoft.com/library/ff393707.aspx)                                | x       | x   | x   |
| [niveau d’authentification = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393709.aspx)                         | x       | x   | x   |
| se connecter à la console = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| désactiver les paramètres du curseur = i:&lt;0 ou 1&gt;                      | x       | x   | x   |
| désactiver le glissement de fenêtre complet = i:&lt;0 ou 1&gt;                     | x       | x   | x   |
| désactiver le menu anims = i:&lt;0 ou 1&gt;                           | x       | x   | x   |
| désactiver les thèmes = i:&lt;0 ou 1&gt;                               | x       | x   | x   |
| désactiver le papier peint = i:&lt;0 ou 1&gt;                            | x       | x   | x   |
| [drivestoredirect = s: *](https://technet.microsoft.com/library/ff393728(v=ws.10).aspx) (c’est la seule valeur prise en charge) | x       | x   |     |
| [desktopheight = i:&lt;valeur en pixels&gt;](https://technet.microsoft.com/library/ff393702.aspx)                       |         | x   |     |
| [desktopwidth = i:&lt;valeur en pixels&gt;](https://technet.microsoft.com/library/ff393697.aspx)                        |         | x   |     |
| [domaine = s:&lt;chaîne&gt;](https://technet.microsoft.com/library/ff393673.aspx)                           | x | x | x |
| [adresse complète = s:&lt;chaîne&gt;](https://technet.microsoft.com/library/ff393661.aspx)                     | x | x | x |
| gatewayhostname = s:&lt;chaîne&gt;                  | x | x | x |
| [gatewayusagemethod = i:&lt;1 ou 2&gt;](https://msdn.microsoft.com/aa381329.aspx)               | x | x | x |
| [demander les informations d’identification sur le client = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393660(v=ws.10).aspx) |   | x |   |
| [loadbalanceinfo = s:&lt;chaîne&gt;](https://technet.microsoft.com/library/ff393684.aspx)                  | x | x | x |
| [redirectprinters = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393671(v=ws.10).aspx)                 |   | x |   |
| remoteapplicationcmdline = s:&lt;chaîne&gt;         | x | x | x |
| remoteapplicationmode = i:&lt;0 ou 1&gt;            | x | x | x |
| remoteapplicationprogram = s:&lt;chaîne&gt;         | x | x | x |
| répertoire de travail Shell = s:&lt;chaîne&gt;          | x | x | x |
| Utiliser le nom de serveur redirection = i:&lt;0 ou 1&gt;      | x | x | x |
| [nom d’utilisateur = s:&lt;chaîne&gt;](https://technet.microsoft.com/library/ff393678.aspx)                         | x | x | x |
| [id du mode écran = i:&lt;1 ou 2&gt;](https://technet.microsoft.com/library/ff393692.aspx)                   |   | x |   |
| [session bpp = i:&lt;8, 15, 16, 24 ou 32&gt;](https://technet.microsoft.com/library/ff393680.aspx)        |   | x |   |
| [Utilisez multimon = i:&lt;0 ou 1&gt;](https://technet.microsoft.com/library/ff393695(v=ws.10).aspx)          |   | x |   |
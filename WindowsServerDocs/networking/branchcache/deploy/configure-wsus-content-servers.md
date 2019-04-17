---
title: Configurer les serveurs de contenu WindowsServerUpdateServices (WSUS)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8200a0905f7bc5c403288a22faece5f84eac8af9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurer les serveurs de contenu WindowsServerUpdateServices (WSUS)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Après avoir installé la fonctionnalité BranchCache et démarrer le service BranchCache, les serveurs WSUS doivent être configurés pour stocker les fichiers de mise à jour sur l’ordinateur local. 

Lorsque vous configurez les serveurs WSUS pour stocker les fichiers de mise à jour sur l’ordinateur local, les métadonnées de mise à jour et les fichiers de mise à jour sont téléchargées par et stockés directement sur le serveur WSUS. Cela garantit que les ordinateurs clients BranchCache reçoivent les fichiers de mise à jour de produit Microsoft à partir du serveur WSUS plutôt que directement le Website de MicrosoftUpdate.  
  
Pour plus d’informations sur la synchronisation WSUS, voir [configuration des synchronisations de mise à jour](https://technet.microsoft.com/en-us/library/mt612311.aspx)  
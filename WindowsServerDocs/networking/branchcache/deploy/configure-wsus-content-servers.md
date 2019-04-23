---
title: Configurer des serveurs de contenu Windows Server Update Services (WSUS)
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e8576282be92f02daf716da82ea75eddc755ee5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873840"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurer des serveurs de contenu Windows Server Update Services (WSUS)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après l’installation de la fonctionnalité BranchCache et démarrage du service BranchCache, les serveurs WSUS doivent être configurés pour stocker les fichiers de mise à jour sur l’ordinateur local. 

Lorsque vous configurez des serveurs WSUS pour stocker des fichiers de mise à jour sur l'ordinateur local, les métadonnées de mise à jour et les fichiers de mise à jour sont téléchargés et stockés directement sur le serveur WSUS. Cela permet de s'assurer que les ordinateurs clients BranchCache reçoivent les fichiers de mise à jour des produits Microsoft du serveur WSUS plutôt que directement du site Web Microsoft Update.  
  
Pour plus d’informations sur la synchronisation de WSUS, consultez [configuration des synchronisations de mise à jour](https://technet.microsoft.com/library/mt612311.aspx)  
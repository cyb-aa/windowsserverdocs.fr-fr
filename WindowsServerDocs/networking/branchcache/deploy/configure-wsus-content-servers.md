---
title: Configurer des serveurs de contenu Windows Server Update Services (WSUS)
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante du réseau étendu dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0836c91948b13d2e6bac540294a55cb49523158d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406416"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurer des serveurs de contenu Windows Server Update Services (WSUS)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Après l’installation de la fonctionnalité BranchCache et le démarrage du service BranchCache, les serveurs WSUS doivent être configurés pour stocker les fichiers de mise à jour sur l’ordinateur local. 

Lorsque vous configurez des serveurs WSUS pour stocker des fichiers de mise à jour sur l'ordinateur local, les métadonnées de mise à jour et les fichiers de mise à jour sont téléchargés et stockés directement sur le serveur WSUS. Cela permet de s'assurer que les ordinateurs clients BranchCache reçoivent les fichiers de mise à jour des produits Microsoft du serveur WSUS plutôt que directement du site Web Microsoft Update.  
  
Pour plus d’informations sur la synchronisation WSUS, voir [Configuration des synchronisations de mise à jour](https://technet.microsoft.com/library/mt612311.aspx)  
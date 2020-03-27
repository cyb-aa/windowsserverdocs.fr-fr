---
title: Configurer des serveurs de contenu Windows Server Update Services (WSUS)
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante du réseau étendu dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 9724aa8d-e4ae-404c-bee6-cef1534cd3ca
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2993c78e85609ac720fd208971dda7ed67a3610d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319162"
---
# <a name="configure-windows-server-update-services-wsus-content-servers"></a>Configurer des serveurs de contenu Windows Server Update Services (WSUS)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Après l’installation de la fonctionnalité BranchCache et le démarrage du service BranchCache, les serveurs WSUS doivent être configurés pour stocker les fichiers de mise à jour sur l’ordinateur local. 

Lorsque vous configurez des serveurs WSUS pour stocker des fichiers de mise à jour sur l'ordinateur local, les métadonnées de mise à jour et les fichiers de mise à jour sont téléchargés et stockés directement sur le serveur WSUS. Cela permet de s'assurer que les ordinateurs clients BranchCache reçoivent les fichiers de mise à jour des produits Microsoft du serveur WSUS plutôt que directement du site Web Microsoft Update.  
  
Pour plus d’informations sur la synchronisation WSUS, voir [Configuration des synchronisations de mise à jour](https://technet.microsoft.com/library/mt612311.aspx)  
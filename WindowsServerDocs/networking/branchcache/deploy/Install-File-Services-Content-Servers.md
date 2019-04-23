---
title: Installer des serveurs de contenu Services de fichiers
description: Cette rubrique fait partie de BranchCache déploiement Guide pour Windows Server 2016, qui montre comment déployer BranchCache en mode cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les succursales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 496c0c1408c64216f29a31d5b22d3d9b48d4f44c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855060"
---
# <a name="install-file-services-content-servers"></a>Installer des serveurs de contenu Services de fichiers

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Pour déployer des serveurs de contenu qui exécutent le rôle serveur Services de fichiers, vous devez installer le service de rôle BranchCache pour fichiers réseau du rôle serveur Services de fichiers. En outre, vous devez activer BranchCache sur les partages de fichiers selon vos besoins.  
  
Pendant la configuration du serveur de contenu, vous pouvez autoriser la publication BranchCache de contenu pour tous les partages de fichiers ou vous pouvez sélectionner un sous-ensemble de partages de fichiers à publier.  
  
> [!NOTE]  
> Lorsque vous déployez un serveur de fichiers activée de BranchCache ou Web comme serveur de contenu, informations de contenu sont désormais calculées en mode hors connexion, bien avant un client BranchCache demande un fichier. En raison de cette amélioration, il est inutile de configurer la publication de hachages pour les serveurs de contenu, comme vous l’avez fait dans la version précédente de BranchCache.  
>   
> Cette génération de hachage automatique fournit des performances plus rapides et plus grandes économies de bande passante, car les informations de contenu sont prêtes pour le premier client qui demande le contenu et les calculs ont déjà été effectuées.  
  
Consultez les rubriques suivantes pour déployer des serveurs de contenu.  
  
-   [Configurer le rôle de serveur Services de fichiers](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Activer la Publication de hachages pour les serveurs de fichiers](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Activer BranchCache sur un partage de fichiers &#40;facultatif&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  



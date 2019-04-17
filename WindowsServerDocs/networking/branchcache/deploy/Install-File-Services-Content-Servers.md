---
title: Installer les serveurs de contenu de Services de fichiers
description: Cette rubrique fait partie de la BranchCache déploiement Guide pour Windows Server2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé d’optimiser l’utilisation de la bande passante réseau étendu dans les filiales
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7036323e7cc31bd14be8025b6064a806fb45ef19
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="install-file-services-content-servers"></a>Installer les serveurs de contenu de Services de fichiers

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Pour déployer des serveurs qui exécutent le rôle de serveur Services de fichiers de contenu, vous devez installer le service de rôle fichiers réseau du rôle de serveur Services de fichiers BranchCache pour. En outre, vous devez activer BranchCache sur les partages de fichiers en fonction de vos besoins.  
  
Lors de la configuration du serveur de contenu, vous pouvez autoriser la publication BranchCache de contenu pour tous les partages de fichiers, ou vous pouvez sélectionner un sous-ensemble de partages de fichiers à publier.  
  
> [!NOTE]  
> Lorsque vous déployez un serveur de fichiers activé BranchCache ou le serveur Web comme un serveur de contenu, les informations de contenu sont désormais calculées hors connexion, bien avant un client BranchCache demande un fichier. En raison de cette amélioration, il est inutile de configurer la publication de hachages pour les serveurs de contenu, comme vous l’avez fait dans la version précédente de BranchCache.  
>   
> Cette génération automatique de hachage fournit des performances plus rapides et plus d’économies de bande passante, car les informations de contenu sont prêtes pour le premier client qui demande le contenu et les calculs ont déjà été effectuées.  
  
Consultez les rubriques suivantes pour déployer des serveurs de contenu.  
  
-   [Configurer le rôle de serveur Services de fichiers](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Activer la publication de hachages pour les serveurs de fichiers](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Activer BranchCache sur un partage de fichiers et #40; facultatif et #41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  



---
title: Installer des serveurs de contenu Services de fichiers
description: Cette rubrique fait partie du Guide de déploiement BranchCache pour Windows Server 2016, qui montre comment déployer BranchCache en mode de cache distribué et hébergé pour optimiser l’utilisation de la bande passante WAN dans les filiales.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 74b0a5ed-dc20-4974-9d4b-2426987a01a1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c55f8df57ed98d13d6d0d6d2a281edfb55883bea
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356555"
---
# <a name="install-file-services-content-servers"></a>Installer des serveurs de contenu Services de fichiers

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Pour déployer des serveurs de contenu qui exécutent le rôle serveur Services de fichiers, vous devez installer le service de rôle BranchCache pour fichiers réseau du rôle serveur Services de fichiers. En outre, vous devez activer BranchCache sur les partages de fichiers en fonction de vos besoins.  
  
Pendant la configuration du serveur de contenu, vous pouvez autoriser la publication BranchCache de contenu pour tous les partages de fichiers ou vous pouvez sélectionner un sous-ensemble de partages de fichiers à publier.  
  
> [!NOTE]  
> Lorsque vous déployez un serveur de fichiers ou un serveur Web prenant en charge BranchCache comme serveur de contenu, les informations de contenu sont maintenant calculées hors connexion, bien avant qu’un client BranchCache demande un fichier. En raison de cette amélioration, vous n’avez pas besoin de configurer la publication de hachages pour les serveurs de contenu, comme vous l’avez fait dans la version précédente de BranchCache.  
>   
> Cette génération de hachage automatique offre des performances plus rapides et des économies de bande passante, car les informations de contenu sont prêtes pour le premier client qui demande le contenu et les calculs ont déjà été effectués.  
  
Consultez les rubriques suivantes pour déployer des serveurs de contenu.  
  
-   [Configurer le rôle serveur Services de fichiers](../../branchcache/deploy/Configure-the-File-Services-server-role.md)  
  
-   [Activer la publication de hachages pour les serveurs de fichiers](../../branchcache/deploy/Enable-Hash-Publication-for-File-Servers.md)  
  
-   [Activer BranchCache sur un partage &#40;de fichiers facultatif&#41;](../../branchcache/deploy/enable-bc-on-file-share.md)  
  



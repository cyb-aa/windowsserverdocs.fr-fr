---
title: Étape 4 vérifier le déploiement Multisite
description: Cette rubrique fait partie du guide de déploiement de plusieurs serveurs d’accès distant dans un déploiement Multisite dans Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17b944bd0e2c13f9a3d324eeda09c67b110ce49d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821850"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Étape 4 vérifier le déploiement Multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement multisite de l’accès à distance.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Pour vérifier l’accès aux ressources internes via le déploiement multisite  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
3.  Tester la connectivité via chaque serveur dans le déploiement multisite par la désactivation ou la déconnexion du réseau externe, une seule des serveurs d’accès à distance. Sur l’ordinateur client, tentez d’accéder aux ressources d’entreprise. Répéter le test sur un autre serveur multisite. Il peut prendre jusqu'à 10 minutes pour l’ordinateur client pour se connecter à un nouveau point d’entrée. Il s’agit, car la détection est désactivée pendant 10 minutes pour un point d’entrée, une fois qu’il est considéré comme inaccessible, afin d’optimiser la bande passante et d’autonomie. Ou bien, vous pouvez basculer entre les points d’entrée différents manuellement en choisissant le point d’entrée souhaité dans la zone de liste déroulante indiquée lors de l’exécution **daprop.exe**.  
  
    Doit pouvoir accéder à toutes les ressources d’entreprise via chaque serveur multisite.  
  
4.  Se connecter à Windows 7&reg; ordinateur client au siège de réseau et d’obtenir la stratégie de groupe.  
  
5.  Connecter l’ordinateur client Windows 7 au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
6.  Tester la connectivité pour les clients Windows 7 via chaque serveur dans le déploiement multisite en accédant à la console Active Directory Users and Computers, puis en déplaçant l’ordinateur client au groupe de sécurité qui correspond à chaque serveur. Une fois la réplication des modifications dans tout le domaine, redémarrez l’ordinateur client lorsque vous êtes connecté au réseau d’entreprise pour obtenir la nouvelle stratégie de groupe. Tentative d’accéder aux ressources d’entreprise. Répéter le test sur un autre serveur multisite.  
  
    Doit pouvoir accéder à toutes les ressources d’entreprise via chaque serveur multisite.  
  
    Dans un environnement de production cette méthode n’est peut-être pas possible en raison de la quantité de temps nécessaire pour que les modifications soient répliquées sur le domaine. Voulez-vous forcer la réplication lorsque cela est possible. Test peut également être effectué à partir de plusieurs ordinateurs clients Windows 7 différents qui sont déjà membres des différents groupes de sécurité de Windows 7 dans le déploiement multisite.  
  



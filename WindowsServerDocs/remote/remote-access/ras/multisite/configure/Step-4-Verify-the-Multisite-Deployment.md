---
title: Étape 4 vérifier le déploiement multisite
description: Cette rubrique fait partie du guide déployer plusieurs serveurs d’accès à distance dans un déploiement multisite dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 1f17f85104b59052de2e1accc20adb2579ac79f4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858352"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Étape 4 vérifier le déploiement multisite

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique explique comment vérifier que vous avez correctement configuré votre déploiement multisite de l’accès à distance.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Pour vérifier l’accès aux ressources internes via le déploiement multisite  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
3.  Testez la connectivité via chaque serveur dans le déploiement multisite en désactivant ou en déconnectant le réseau externe, à l’exception de l’un des serveurs d’accès à distance. Sur l’ordinateur client, tentez d’accéder aux ressources de l’entreprise. Répétez le test sur un autre serveur multisite. La connexion de l’ordinateur client au nouveau point d’entrée peut prendre jusqu’à 10 minutes. Cela est dû au fait que la détection est désactivée pendant 10 minutes pour un point d’entrée après qu’elle est jugée inaccessible, afin d’optimiser la bande passante et la durée de vie de la batterie. Vous pouvez également basculer entre les différents points d’entrée manuellement en choisissant le point d’entrée souhaité à partir de la zone de liste déroulante affichée lors de l’exécution de **daprop. exe**.  
  
    Vous devez être en mesure d’accéder à toutes les ressources d’entreprise par le biais de chaque serveur multisite.  
  
4.  Connectez un ordinateur client Windows 7&reg; au réseau d’entreprise et obtenez la stratégie de groupe.  
  
5.  Connectez l’ordinateur client Windows 7 au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
6.  Testez la connectivité pour les clients Windows 7 via chaque serveur dans le déploiement multisite en accédant à la console utilisateurs et ordinateurs Active Directory et en déplaçant l’ordinateur client vers le groupe de sécurité qui correspond à chaque serveur. Une fois que les modifications ont été répliquées dans l’ensemble du domaine, redémarrez l’ordinateur client tout en étant connecté au réseau d’entreprise pour obtenir la nouvelle stratégie de groupe. Essayez d’accéder aux ressources de l’entreprise. Répétez le test sur un autre serveur multisite.  
  
    Vous devez être en mesure d’accéder à toutes les ressources d’entreprise par le biais de chaque serveur multisite.  
  
    Dans un environnement de production, cette méthode peut ne pas être possible en raison de la durée nécessaire à la réplication des modifications dans l’ensemble du domaine. Vous souhaiterez peut-être forcer la réplication dans la mesure du possible. Les tests peuvent également être effectués à partir de plusieurs ordinateurs clients Windows 7 déjà membres des différents groupes de sécurité Windows 7 dans le déploiement multisite.  
  



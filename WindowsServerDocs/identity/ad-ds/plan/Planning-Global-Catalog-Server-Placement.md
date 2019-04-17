---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planification du Placement de serveur de catalogue Global
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c32393640e929adf9681f511ed3707b85a3784db
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="planning-global-catalog-server-placement"></a>Planification du Placement de serveur de catalogue Global

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La sélection élective de catalogue global requiert une planification, sauf si vous disposez d’une forêt à domaine unique. Dans une forêt à domaine unique, configurez tous les contrôleurs de domaine en tant que serveurs de catalogue global. Étant donné que chaque contrôleur de domaine stocke la partition d’annuaire de domaine uniquement dans la forêt, la configuration de chaque contrôleur de domaine en tant que serveur de catalogue global ne nécessite pas l’utilisation d’espace disque supplémentaire, l’utilisation du processeur ni le trafic de réplication. Dans une forêt à domaine unique, tous les contrôleurs de domaine agissent en tant que serveurs de catalogue global virtuel; autrement dit, ils tous répondent tout l’authentification ou de demande de service. Cette condition spéciale pour les forêts de domaine unique est normal. Demandes d’authentification ne nécessitent pas contacter un serveur de catalogue global comme ils le font lorsqu’il existe plusieurs domaines, et un utilisateur peut être un membre d’un groupe universel qui existe dans un autre domaine. Toutefois, seuls les contrôleurs de domaine sont désignés comme serveurs de catalogue global peuvent répondre aux requêtes de catalogue global sur le port 3268 du catalogue global. Pour simplifier l’administration dans ce scénario et garantir des réponses cohérentes, la désignation de tous les contrôleurs de domaine en tant que serveurs de catalogue global évite le problème sur le domaine dans lequel les contrôleurs peuvent répondre aux requêtes de catalogue global. Plus précisément, chaque fois qu’un utilisateur utilise des personnes Start\Search\For ou rechercher les imprimantes ou développe les groupes universels, ces demandes Accédez uniquement dans le catalogue global.  
  
Dans les forêts de plusieurs domaines, serveurs de catalogue global facilitent les demandes d’ouverture de session utilisateur et les recherches à l’échelle de la forêt. L’illustration suivante montre comment déterminer les emplacements qui nécessitent des serveurs de catalogue global.  
  
![Planification de l’emplacement dans le catalogue global](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Dans la plupart des cas, il est recommandé d’inclure le catalogue global lors de l’installation de nouveaux contrôleurs de domaine. Les exceptions suivantes s’appliquent:  
  
-   La bande passante limitée: dans les sites distants, si la liaison de réseau (étendu WAN) étendu entre le site distant et le site hub est limitée, vous pouvez utiliser la mise en cache de l’appartenance au groupe universel dans le site distant pour prendre en charge les besoins d’ouverture de session des utilisateurs dans le site.  
  
-   Incompatibilité du rôle de maître d’opérations de l’infrastructure: ne placez pas le catalogue global sur un contrôleur de domaine qui héberge le rôle de maître d’opérations infrastructure dans le domaine, sauf si tous les contrôleurs de domaine dans le domaine sont des serveurs de catalogue global ou de la forêt n'a qu’un seul domaine.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Ajout de serveurs de catalogue global selon les besoins de l’application  
Certaines applications, telles que MicrosoftExchange, Message Queuing (MSMQ) et applications à l’aide de DCOM ne pas fournir une réponse adéquate sur les liaisons WAN latentes et donc besoin d’une infrastructure de haut niveau de catalogue global pour fournir des temps de réponse faible. Déterminer si toutes les applications qui exécutent mal via une liaison réseau étendue lente sont en cours d’exécution dans des emplacements ou si les emplacements requièrent MicrosoftExchange Server. Si vos emplacements incluent des applications qui ne pas envoyer de réponse adéquate sur une liaison WAN, vous devez placer un serveur de catalogue global à l’emplacement afin de réduire la latence des requêtes.  
  
> [!NOTE]  
> Read-only contrôleurs de domaine (RODC) peuvent être promus à l’état de serveur de catalogue global. Toutefois, certaines applications d’annuaire ne peut pas prendre en charge un RODC en tant que serveur de catalogue global. Par exemple, aucune version de MicrosoftExchange Server n’utilise RODC. Cependant, MicrosoftExchange Server fonctionne dans les environnements qui incluent les RODC, tant que contrôleurs de domaine accessibles en écriture sont disponibles. Exchange Server2007 ignore efficacement le RODC. Exchange Server2003 ignore également les RODC dans des conditions par défaut dans lequel les composants Exchange détectent automatiquement les contrôleurs de domaine disponibles. Aucune modification n’ont été apportées à Exchange Server2003 pour le rendre prenant en charge des serveurs d’annuaire en lecture seule. Par conséquent, vous essayez de forcer les services Exchange Server2003 et les outils de gestion pour utiliser le RODC peut entraîner un comportement imprévisible.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Ajout de serveurs de catalogue global pour un grand nombre d’utilisateurs  
Placer des serveurs de catalogue global à tous les emplacements qui contiennent plus de 100utilisateurs afin de réduire la surcharge des liaisons WAN réseau et pour éviter la perte de productivité en cas de défaillance de la liaison WAN.  
  
## <a name="using-highly-available-bandwidth"></a>À l’aide de la bande passante hautement disponible  
Vous n’avez pas besoin de placer un catalogue global à un emplacement qui n’inclut pas les applications qui nécessitent un serveur de catalogue global, comprend moins de 100utilisateurs et est également connecté à un autre emplacement qui inclut un serveur de catalogue global par une liaison WAN est disponible pour les Services de domaine ActiveDirectory (ADDS) à 100%. Dans ce cas, les utilisateurs peuvent accéder le serveur de catalogue global sur la liaison WAN.  
  
Les utilisateurs itinérants doivent contacter les serveurs de catalogue global, chaque fois qu’ils se connectent pour la première fois à n’importe quel emplacement. Si l’heure d’ouverture de session sur la liaison WAN est inacceptable, placez un catalogue global à un emplacement visité par un grand nombre d’utilisateurs itinérants.  
  
## <a name="enabling-universal-group-membership-caching"></a>L’activation de la mise en cache de l’appartenance au groupe universel  
Pour les emplacements qui incluent le moins de 100utilisateurs et qui n’incluent pas d’un grand nombre d’utilisateurs itinérants ou d’applications qui nécessitent un serveur de catalogue global, vous pouvez déployer des contrôleurs de domaine qui exécutent Windows Server2008 et activer la mise en cache de l’appartenance au groupe universel. Assurez-vous que les serveurs de catalogue global ne sont pas plus d’un saut de réplication du contrôleur de domaine sur lequel la mise en cache de l’appartenance au groupe universel est activée afin que les informations du groupe universel dans le cache peuvent être actualisées. Pour plus d’informations sur comment universelle fonctionne de la mise en cache de groupe, voir Global catalogue fonctionnement ([https://go.microsoft.com/fwlink/?LinkId=107063](https://go.microsoft.com/fwlink/?LinkId=107063)).  
  
Pour une feuille de calcul pour vous aider dans la documentation sur lequel vous envisagez de placer des serveurs de catalogue global et les contrôleurs de domaine avec le groupe universel de mise en cache activée, voir tâche identificateurs d’applet pour Windows Server2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), téléchargez < DICT__Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and open Domain Contrôleur la sélection élective (DSSTOPO_4.doc). Consultez les informations sur les emplacements dans laquelle vous devez placer les serveurs de catalogue global lorsque vous déployez le domaine racine de forêt et les domaines régionaux.  
  



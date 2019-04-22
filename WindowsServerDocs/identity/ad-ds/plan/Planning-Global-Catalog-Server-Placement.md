---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planification de l’emplacement des serveurs de catalogue global
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: aefed1a2ed0d346f75ad4d135f8c0ae314fd7fc7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820680"
---
# <a name="planning-global-catalog-server-placement"></a>Planification de l’emplacement des serveurs de catalogue global

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Placement de catalogue global nécessite une planification, sauf si vous avez une forêt à domaine unique. Dans une forêt à domaine unique, configurez tous les contrôleurs de domaine en tant que serveurs de catalogue global. Étant donné que chaque contrôleur de domaine stocke la partition d’annuaire de domaine uniquement dans la forêt, la configuration de chaque contrôleur de domaine en tant que serveur de catalogue global ne nécessite pas l’espace disque supplémentaire, l’utilisation du processeur ni le trafic de réplication. Dans une forêt à domaine unique, tous les contrôleurs de domaine agissent en tant que serveurs de catalogue global virtuel ; Autrement dit, ils peuvent tous répondent aucune authentification ou d’une demande de service. Cette condition spéciale pour les forêts à domaine unique est normal. Demandes d’authentification ne nécessitent pas de contacter un serveur de catalogue global comme ils le font lorsqu’il existe plusieurs domaines, et un utilisateur peut être un membre d’un groupe universel qui existe dans un autre domaine. Toutefois, seuls les contrôleurs de domaine qui sont désignés comme serveurs de catalogue global peuvent répondre aux requêtes de catalogue global sur le port 3268 du catalogue global. Pour simplifier l’administration dans ce scénario et vous assurer de réponses cohérents, qui désigne tous les contrôleurs de domaine en tant que serveurs de catalogue global élimine le problème sur le domaine dans lequel les contrôleurs peuvent répondre aux requêtes de catalogue global. Plus précisément, chaque fois qu’un utilisateur utilise Start\Search\For personnes ou rechercher les imprimantes ou développe des groupes universels, ces demandes passent uniquement dans le catalogue global.  
  
Dans les forêts de plusieurs domaines, serveurs de catalogue global facilitent les demandes d’ouverture de session utilisateur et les recherches de forêt. L’illustration suivante montre comment déterminer les emplacements qui nécessitent des serveurs de catalogue global.  
  
![planification de l’emplacement du catalogue global](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Dans la plupart des cas, il est recommandé d’inclure le catalogue global lorsque vous installez de nouveaux contrôleurs de domaine. Les exceptions suivantes s’appliquent :  
  
- Bande passante limitée : Dans les sites distants, si la liaison de réseau (étendu WAN) étendu entre le site distant et le site hub est limitée, vous pouvez utiliser la mise en cache de l’appartenance au groupe universel dans le site distant pour répondre aux besoins des utilisateurs dans le site d’ouverture de session.  
- Incompatibilité de rôle de maître d’opérations de infrastructure : Ne placez pas le catalogue global sur un contrôleur de domaine que les opérations de l’infrastructure d’hôtes rôle du maître dans le domaine, sauf si tous les contrôleurs de domaine dans le domaine sont des serveurs de catalogue global ou de la forêt n'a qu’un seul domaine.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Ajout de serveurs de catalogue global en fonction des exigences de l’application

Certaines applications, telles que Microsoft Exchange, Message Queuing (également appelé MSMQ) et applications qui utilisent DCOM ne pas fournir une réponse adéquate sur des liaisons WAN latentes et donc besoin d’une infrastructure de catalogue global hautement disponible pour fournir une requête faible temps de latence. Déterminer si toutes les applications qui exécutent mal sur une liaison WAN lente sont en cours d’exécution dans des emplacements ou si les emplacements nécessitent Microsoft Exchange Server. Si vos emplacements incluent des applications qui ne pas envoyer de réponse adéquate sur une liaison WAN, vous devez placer un serveur de catalogue global à l’emplacement pour réduire la latence de requête.  
  
> [!NOTE]  
> Contrôleurs de domaine en lecture seule (RODC) peuvent être promus avec succès de l’état de serveur de catalogue global. Toutefois, certaines applications d’annuaire ne peut pas prendre en charge un RODC en tant que serveur de catalogue global. Par exemple, aucune version de Microsoft Exchange Server n’utilise ces contrôleurs. Toutefois, Microsoft Exchange Server fonctionne dans les environnements qui incluent les RODC, tant que contrôleurs de domaine accessible en écriture sont disponibles. Exchange Server 2007 ignore efficacement les RODC. Exchange Server 2003 ignore également les RODC dans les conditions par défaut dans lequel composants Exchange détectent automatiquement les contrôleurs de domaine disponibles. Aucune modification ont été apportées à Exchange Server 2003 pour le rendre prenant en charge des serveurs d’annuaire en lecture seule. Par conséquent, vous essayez de forcer les services Exchange Server 2003 et les outils de gestion pour utiliser les RODC peut entraîner un comportement imprévisible.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Ajout de serveurs de catalogue global pour un grand nombre d’utilisateurs

Placer les serveurs de catalogue global à tous les emplacements qui contiennent plus de 100 utilisateurs afin de réduire la congestion des liaisons WAN de réseau et pour éviter la perte de productivité en cas d’échec de la liaison réseau étendu.  
  
## <a name="using-highly-available-bandwidth"></a>À l’aide de la bande passante hautement disponible

Vous n’avez pas besoin de placer un catalogue global à un emplacement qui n’inclut pas les applications qui requièrent un serveur de catalogue global, inclut moins de 100 utilisateurs et est également connecté à un autre emplacement qui inclut un serveur de catalogue global par une liaison réseau étendu est de 100 pour cent un disponibles pour les Services de domaine Active Directory (AD DS). Dans ce cas, les utilisateurs peuvent accéder le serveur de catalogue global sur la liaison WAN.  
  
Les utilisateurs itinérants doivent contacter les serveurs de catalogue global, chaque fois qu’ils se connectent pour la première fois à n’importe quel emplacement. Si l’heure d’ouverture de session sur la liaison WAN est inacceptable, placez un catalogue global à un emplacement qui est visité par un grand nombre d’utilisateurs itinérants.  
  
## <a name="enabling-universal-group-membership-caching"></a>L’activation de la mise en cache de l’appartenance au groupe universel

Pour les emplacements qui comportent moins de 100 utilisateurs et qui n’incluent pas un grand nombre d’utilisateurs itinérants ou d’applications qui requièrent un serveur de catalogue global, vous pouvez déployer des contrôleurs de domaine qui exécutent Windows Server 2008 et activer l’appartenance au groupe universel la mise en cache. Assurez-vous que les serveurs de catalogue global ne sont pas plus d’un tronçon de réplication à partir du contrôleur de domaine sur lequel la mise en cache de l’appartenance au groupe universel est activée afin que les informations de groupe universel dans le cache peuvent être actualisées. Pour plus d’informations sur comment universelle works de mise en cache de groupe, consultez l’article [Global Catalog fonctionnement](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Pour une feuille de calcul pour vous aider à documenter dans lequel vous envisagez de placer des serveurs de catalogue global et les contrôleurs de domaine avec la mise en cache de groupe universel activé, consultez [travail aides pour Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), téléchargez Job_Aids_ Designing_and_Deploying_Directory_and_Security_Services.zip et Placement des contrôleurs de domaine ouvert (DSSTOPO_4.doc). Consultez les informations sur les emplacements dans lesquels vous devez placer les serveurs de catalogue global lorsque vous déployez le domaine racine de forêt et domaines régionaux.  

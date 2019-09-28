---
ms.assetid: 407d5e81-c04c-4275-9ae9-35f65b4a371a
title: Planification de l’emplacement des serveurs de catalogue global
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: fb61d917300e957534a688b73efd7e193d24ff6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408740"
---
# <a name="planning-global-catalog-server-placement"></a>Planification de l’emplacement des serveurs de catalogue global

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

L’emplacement du catalogue global nécessite une planification, sauf si vous avez une forêt à domaine unique. Dans une forêt à domaine unique, configurez tous les contrôleurs de domaine en tant que serveurs de catalogue global. Étant donné que chaque contrôleur de domaine stocke la seule partition d’annuaire de domaine dans la forêt, la configuration de chaque contrôleur de domaine en tant que serveur de catalogue global ne nécessite pas d’utilisation de l’espace disque supplémentaire, de l’utilisation du processeur ou du trafic de réplication. Dans une forêt à domaine unique, tous les contrôleurs de domaine jouent le rôle de serveurs de catalogue global virtuels ; autrement dit, ils peuvent répondre à toute demande d’authentification ou de service. Cette condition spéciale pour les forêts à un seul domaine est par défaut. Les demandes d’authentification ne nécessitent pas de contacter un serveur de catalogue global comme c’est le cas lorsqu’il y a plusieurs domaines, et un utilisateur peut être membre d’un groupe universel qui existe dans un autre domaine. Toutefois, seuls les contrôleurs de domaine désignés comme serveurs de catalogue global peuvent répondre aux requêtes de catalogue global sur le port de catalogue global 3268. Pour simplifier l’administration de ce scénario et garantir la cohérence des réponses, la désignation de tous les contrôleurs de domaine en tant que serveurs de catalogue global élimine les problèmes auxquels les contrôleurs de domaine peuvent répondre aux requêtes de catalogue global. Plus précisément, chaque fois qu’un utilisateur utilise des personnes Start\Search\For ou recherche des imprimantes ou développe des groupes universels, ces demandes sont dirigées uniquement vers le catalogue global.  
  
Dans les forêts à plusieurs domaines, les serveurs de catalogue global facilitent les demandes d’ouverture de session utilisateur et les recherches à l’échelle de la forêt. L’illustration suivante montre comment déterminer les emplacements qui requièrent des serveurs de catalogue global.  
  
![planification de l’emplacement gc](media/Planning-Global-Catalog-Server-Placement/8fc4777c-47b6-4ee7-b8ad-a04e7c5ee67f.gif)  
  
Dans la plupart des cas, il est recommandé d’inclure le catalogue global lorsque vous installez de nouveaux contrôleurs de domaine. Les exceptions suivantes s’appliquent :  
  
- Bande passante limitée : Dans les sites distants, si la liaison de réseau étendu (WAN) entre le site distant et le site Hub est limitée, vous pouvez utiliser la mise en cache de l’appartenance au groupe universel dans le site distant pour répondre aux besoins d’ouverture de session des utilisateurs dans le site.  
- Incompatibilité du rôle du maître d’opérations d’infrastructure : Ne placez pas le catalogue global sur un contrôleur de domaine qui héberge le rôle de maître d’opérations d’infrastructure dans le domaine, sauf si tous les contrôleurs de domaine du domaine sont des serveurs de catalogue global ou si la forêt ne possède qu’un seul domaine.  
  
## <a name="adding-global-catalog-servers-based-on-application-requirements"></a>Ajout de serveurs de catalogue global en fonction des exigences de l’application

Certaines applications, telles que Microsoft Exchange, Message Queuing (également appelé MSMQ) et les applications utilisant DCOM, ne fournissent pas de réponse adéquate sur les liaisons WAN latentes et nécessitent donc une infrastructure de catalogue global hautement disponible pour fournir une faible requête latence. Déterminez si des applications qui effectuent des performances médiocres sur une liaison WAN lente sont en cours d’exécution à des emplacements ou si les emplacements nécessitent Microsoft Exchange Server. Si vos emplacements incluent des applications qui ne fournissent pas de réponse adéquate sur une liaison WAN, vous devez placer un serveur de catalogue global à l’emplacement pour réduire la latence des requêtes.  
  
> [!NOTE]  
> Les contrôleurs de domaine en lecture seule (RODC) peuvent être promus avec succès en état de serveur de catalogue global. Toutefois, certaines applications compatibles avec l’annuaire ne peuvent pas prendre en charge un contrôleur de domaine en lecture seule en tant que serveur de catalogue global. Par exemple, aucune version de Microsoft Exchange Server n’utilise RODC. Toutefois, Microsoft Exchange Server fonctionne dans les environnements qui incluent des RODC, à condition qu’il y ait des contrôleurs de domaine accessibles en écriture. Exchange Server 2007 ignore de manière effective les RODC. Exchange Server 2003 ignore également les contrôleurs de domaine en lecture seule dans les conditions par défaut où les composants Exchange détectent automatiquement les contrôleurs de domaine disponibles. Aucune modification n’a été apportée à Exchange Server 2003 pour qu’il prenne en charge les serveurs d’annuaire en lecture seule. Par conséquent, la tentative de forcer les services Exchange Server 2003 et les outils de gestion à utiliser RODC peut entraîner un comportement imprévisible.  
  
## <a name="adding-global-catalog-servers-for-a-large-number-of-users"></a>Ajout de serveurs de catalogue global pour un grand nombre d’utilisateurs

Placez des serveurs de catalogue global à tous les emplacements contenant plus de 100 utilisateurs afin de réduire la congestion des liaisons réseau WAN et d’éviter la perte de productivité en cas de défaillance de la liaison WAN.  
  
## <a name="using-highly-available-bandwidth"></a>Utilisation de la bande passante hautement disponible

Vous n’avez pas besoin de placer un catalogue global dans un emplacement qui n’inclut pas les applications qui requièrent un serveur de catalogue global, qui inclut moins de 100 utilisateurs et qui est également connecté à un autre emplacement qui inclut un serveur de catalogue global par une liaison de réseau étendu de 100% a valide pour Active Directory Domain Services (AD DS). Dans ce cas, les utilisateurs peuvent accéder au serveur de catalogue global par le biais de la liaison de réseau étendu.  
  
Les utilisateurs itinérants doivent contacter les serveurs de catalogue global chaque fois qu’ils ouvrent une session pour la première fois à n’importe quel emplacement. Si la durée d’ouverture de session sur la liaison WAN est inacceptable, placez un catalogue global à un emplacement visité par un grand nombre d’utilisateurs itinérants.  
  
## <a name="enabling-universal-group-membership-caching"></a>Activation de la mise en cache de l’appartenance au groupe universel

Pour les emplacements qui incluent moins de 100 utilisateurs et qui n’incluent pas un grand nombre d’utilisateurs itinérants ou d’applications qui nécessitent un serveur de catalogue global, vous pouvez déployer des contrôleurs de domaine qui exécutent Windows Server 2008 et activer l’appartenance au groupe universel. mettant. Assurez-vous que les serveurs de catalogue global ne sont pas plus d’un tronçon de réplication du contrôleur de domaine sur lequel la mise en cache de l’appartenance au groupe universel est activée afin que les informations de groupe universel dans le cache puissent être actualisées. Pour plus d’informations sur le fonctionnement de la mise en cache du groupe universel, consultez l’article fonctionnement [du catalogue global](https://go.microsoft.com/fwlink/?LinkId=107063).  
  
Pour obtenir une feuille de calcul qui vous aide à documenter l’emplacement où vous envisagez de placer les serveurs de catalogue global et les contrôleurs de domaine avec la mise en cache de groupe universel activée, consultez [Outils d’aide pour le kit de déploiement de Windows Server 2003](https://go.microsoft.com/fwlink/?LinkID=102558), Télécharger Job_Aids_Designing_and_Deploying_ Directory_and_Security_Services. zip et emplacement du contrôleur de domaine ouvert (DSSTOPO_4. doc). Consultez les informations sur les emplacements dans lesquels vous devez placer des serveurs de catalogue global lorsque vous déployez le domaine racine de forêt et les domaines régionaux.  

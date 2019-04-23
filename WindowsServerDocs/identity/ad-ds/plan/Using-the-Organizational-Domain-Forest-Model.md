---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: À l’aide du modèle de forêt de domaines d’organisation
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 876a15dcdd951e0323fb7ddb7be96317f5512f0f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875910"
---
# <a name="using-the-organizational-domain-forest-model"></a>À l’aide du modèle de forêt de domaines d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans le modèle de forêt de domaines de l’organisation, plusieurs groupes autonomes chaque propriétaire d’un domaine au sein d’une forêt. Chaque groupe de contrôle d’administration de service au niveau du domaine, ce qui permet de gérer certains aspects de gestion des services de manière autonome tandis que le propriétaire de la forêt contrôle la gestion des services de niveau de la forêt.  

L’illustration suivante montre un modèle de forêt de domaines de l’organisation.  

![à l’aide du modèle de forêt de domaine org](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomie des services de niveau de domaine

Le modèle de forêt de domaine d’organisation permet la délégation de l’autorité pour la gestion de service au niveau du domaine. Le tableau suivant répertorie les types de gestion des services qui peuvent être contrôlées au niveau du domaine.  

|Type de gestion des services|Tâches associées|  
|------------------------------|--------------------|  
|Gestion des opérations de contrôleur de domaine|-Création et suppression de contrôleurs de domaine<br />-Surveillance du fonctionnement des contrôleurs de domaine<br />-Gestion des services qui s’exécutent sur les contrôleurs de domaine<br />-Sauvegarde et restauration du répertoire|  
|Configuration des paramètres de l’échelle du domaine|-Création de domaine et utilisateur de domaine des stratégies de compte, telles que le mot de passe, Kerberos et les stratégies de verrouillage de compte<br />-Création et application de la stratégie de groupe de l’échelle du domaine|  
|Délégation de l’administration de niveau de données|-Création d’unités d’organisation (UO) et la délégation de l’administration<br />-Réparer des problèmes dans la structure d’unité d’organisation qui propriétaires de l’unité d’organisation n’ont pas de droits d’accès suffisants pour résoudre|  
|Gestion des approbations externes|-Établir des relations d’approbation avec les domaines en dehors de la forêt|  

Autres types de gestion des services, tels que de schéma ou de gestion de topologie de réplication, sont la responsabilité du propriétaire de la forêt.  

## <a name="domain-owner"></a>Propriétaire du domaine

Dans un modèle de forêt de domaines de l’organisation, les propriétaires de domaine sont responsables de tâches de gestion de service au niveau du domaine. Les propriétaires de domaine ont une autorité sur le domaine entier ainsi qu’un accès à tous les autres domaines dans la forêt. Pour cette raison, les propriétaires de domaine doivent être sélectionnées par le propriétaire de la forêt de personnes de confiance.  

Déléguer la gestion de service au niveau du domaine pour le propriétaire d’un domaine, si les conditions suivantes sont remplies :  

- Tous les groupes qui participent à la forêt d’approbation le nouveau propriétaire de domaine et les pratiques de gestion de service du nouveau domaine.  

- Le nouveau propriétaire du domaine approuve le propriétaire de la forêt et tous les autres propriétaires de domaine.  

- Tous les propriétaires de domaine dans la forêt conviennent que le nouveau propriétaire du domaine a gestion des administrateurs de service et les stratégies de sélection et les pratiques qui sont égaux ou plus strict que leurs propres.  

- Tous les propriétaires de domaine dans la forêt conviennent que les contrôleurs de domaine gérés par le nouveau propriétaire de domaine dans le nouveau domaine sont physiquement sécurisés.  

Notez que si une forêt propriétaire délègue au niveau du domaine service la gestion à un propriétaire de domaine, autres groupes peuvent décider de joindre cette forêt si elles ne faites pas confiance à ce propriétaire du domaine.  

Tous les propriétaires de domaine doivent être conscients que si un de ces conditions change à l’avenir, il peut s’avérer nécessaire de déplacer les domaines d’organisation dans un déploiement à forêts multiples.  

> [!NOTE]  
> Une autre façon de réduire les risques de sécurité à un domaine Windows Server 2008 Active Directory consiste à employer la séparation des rôles administrateur, ce qui nécessite le déploiement d’un contrôleur de domaine en lecture seule (RODC) dans votre infrastructure Active Directory. Un RODC est un nouveau type de contrôleur de domaine dans le système d’exploitation Windows Server 2008 qui héberge les partitions en lecture seule de la base de données Active Directory. Avant la version de Windows Server 2008, tout travail de maintenance de serveur sur un contrôleur de domaine devait être effectuée par un administrateur de domaine. Dans Windows Server 2008, vous pouvez déléguer des autorisations d’administrateur local pour un RODC pour n’importe quel utilisateur de domaine sans accorder de droits d’administration pour le domaine ou d’autres contrôleurs de domaine de cet utilisateur. Cela permet à l’utilisateur délégué pour vous connecter à un RODC et effectuer un travail de maintenance, telles que la mise à niveau d’un pilote, sur le serveur. Toutefois, cet utilisateur délégué ne peut pas ouvrir une session sur un autre contrôleur de domaine ou effectuer toute autre tâche d’administration dans le domaine. De cette façon, tout approuvé utilisateur peut être déléguée la possibilité de gérer efficacement le RODC sans compromettre la sécurité du reste du domaine. Pour plus d’informations sur les RODC, consultez [AD DS : Contrôleurs de domaine en lecture seule](https://go.microsoft.com/fwlink/?LinkId=106616).  

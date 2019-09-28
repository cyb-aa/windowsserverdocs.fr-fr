---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: Utilisation du modèle de forêt de domaine d’organisation
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: af5fd2da396ecc27db68d3be8d1c0eda82314d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402443"
---
# <a name="using-the-organizational-domain-forest-model"></a>Utilisation du modèle de forêt de domaine d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Dans le modèle de forêt de domaine d’organisation, plusieurs groupes autonomes disposent chacun d’un domaine au sein d’une forêt. Chaque groupe contrôle l’administration des services au niveau du domaine, ce qui leur permet de gérer certains aspects de la gestion des services de façon autonome, tandis que le propriétaire de la forêt contrôle la gestion des services au niveau de la forêt.  

L’illustration suivante montre un modèle de forêt de domaine d’organisation.  

![utilisation du modèle de forêt de domaine d’organisation](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  

## <a name="domain-level-service-autonomy"></a>Autonomie du service au niveau du domaine

Le modèle de forêt de domaine d’organisation active la délégation d’autorité pour la gestion des services au niveau du domaine. Le tableau suivant répertorie les types de gestion de service qui peuvent être contrôlés au niveau du domaine.  

|Type de gestion de service|Tâches associées|  
|------------------------------|--------------------|  
|Gestion des opérations du contrôleur de domaine|-Création et suppression de contrôleurs de domaine<br />-Surveillance du fonctionnement des contrôleurs de domaine<br />-Gestion des services exécutés sur les contrôleurs de domaine<br />-Sauvegarde et restauration de l’annuaire|  
|Configuration des paramètres à l’ensemble du domaine|-Création de stratégies de compte d’utilisateur de domaine et de domaine, telles que le mot de passe, Kerberos et les stratégies de verrouillage de compte<br />-Création et application d’stratégie de groupe à l’ensemble du domaine|  
|Délégation de l’administration au niveau des données|-Créer des unités d’organisation (UO) et déléguer l’administration<br />-Réparation des problèmes dans la structure de l’unité d’organisation dont les propriétaires d’UO ne disposent pas de droits d’accès suffisants pour corriger|  
|Gestion des approbations externes|-Établissement de relations d’approbation avec des domaines en dehors de la forêt|  

Les autres types de gestion de service, tels que la gestion de la topologie de schéma ou de réplication, sont la responsabilité du propriétaire de la forêt.  

## <a name="domain-owner"></a>Propriétaire du domaine

Dans un modèle de forêt de domaine d’organisation, les propriétaires de domaine sont responsables des tâches de gestion de service au niveau du domaine. Les propriétaires de domaine disposent de l’autorité sur l’ensemble du domaine, ainsi que l’accès à tous les autres domaines de la forêt. Pour cette raison, les propriétaires de domaine doivent être des personnes approuvées sélectionnées par le propriétaire de la forêt.  

Déléguer la gestion des services au niveau du domaine à un propriétaire de domaine, si les conditions suivantes sont remplies :  

- Tous les groupes participant à la forêt approuvent le nouveau propriétaire du domaine et les pratiques de gestion des services du nouveau domaine.  

- Le nouveau propriétaire du domaine approuve le propriétaire de la forêt et tous les autres propriétaires du domaine.  

- Tous les propriétaires de domaines de la forêt conviennent que le nouveau propriétaire du domaine a des stratégies et des pratiques de gestion et de sélection des administrateurs de service qui sont égales ou plus strictes que leur propre propriétaire.  

- Tous les propriétaires de domaine de la forêt conviennent que les contrôleurs de domaine gérés par le nouveau propriétaire de domaine dans le nouveau domaine sont physiquement sécurisés.  

Notez que si un propriétaire de forêt délègue la gestion de service au niveau du domaine à un propriétaire de domaine, d’autres groupes peuvent choisir de ne pas joindre cette forêt s’ils ne font pas confiance à ce propriétaire de domaine.  

Tous les propriétaires de domaine doivent savoir que si l’une de ces conditions change à l’avenir, il peut être nécessaire de déplacer les domaines organisationnels dans un déploiement à plusieurs forêts.  

> [!NOTE]  
> Une autre façon de réduire les risques de sécurité pour un domaine Windows Server 2008 Active Directory consiste à utiliser la séparation des rôles d’administrateur, ce qui nécessite le déploiement d’un contrôleur de domaine en lecture seule (RODC) dans votre infrastructure Active Directory. Un RODC est un nouveau type de contrôleur de domaine dans le système d’exploitation Windows Server 2008 qui héberge des partitions en lecture seule de la base de données Active Directory. Avant la sortie de Windows Server 2008, tout travail de maintenance de serveur sur un contrôleur de domaine devait être effectué par un administrateur de domaine. Dans Windows Server 2008, vous pouvez déléguer des autorisations d’administrateur local pour un RODC à n’importe quel utilisateur de domaine sans accorder à cet utilisateur des droits d’administration pour le domaine ou d’autres contrôleurs de domaine. Cela permet à l’utilisateur délégué de se connecter à un RODC et d’effectuer des tâches de maintenance, telles que la mise à niveau d’un pilote, sur le serveur. Toutefois, cet utilisateur délégué ne peut pas se connecter à un autre contrôleur de domaine ou effectuer d’autres tâches d’administration dans le domaine. De cette façon, tous les utilisateurs approuvés peuvent être délégataires de la capacité à gérer efficacement le RODC sans compromettre la sécurité du reste du domaine. Pour plus d’informations sur les contrôleurs de domaine en lecture seule, voir @no__t 0AD : Contrôleurs de domaine en lecture seule @ no__t-0.  

---
ms.assetid: 093ef1ae-ebc1-490f-9fb1-2c000ce89eb6
title: "L’utilisation du modèle de forêt de domaines d’organisation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 22d871d9157622375619dd90336e597d4bfb3d68
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="using-the-organizational-domain-forest-model"></a>L’utilisation du modèle de forêt de domaines d’organisation

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Dans le modèle de forêt de domaines de l’organisation, plusieurs groupes autonomes chaque possèdent un domaine dans une forêt. Chaque groupe de contrôle d’administration de service au niveau du domaine, ce qui permet de gérer certains aspects de la gestion du service de manière autonome tandis que le propriétaire de la forêt contrôle la gestion des services de niveau de la forêt.  
  
L’illustration suivante montre un modèle de forêt de domaines de l’organisation.  
  
![l’utilisation du modèle de forêt de domaine org](../../media/Using-the-Organizational-Domain-Forest-Model/c50a3c6a-b0e4-43ec-ad62-f05d05f0bbd2.gif)  
  
## <a name="domain-level-service-autonomy"></a>Autonomie des services au niveau du domaine  
Le modèle de forêt de domaines de l’organisation permet la délégation de l’autorité de gestion des services au niveau du domaine. Le tableau suivant répertorie les types de gestion des services qui peuvent être contrôlés au niveau du domaine.  
  
|Type de gestion des services|Tâches associées|  
|------------------------------|--------------------|  
|Gestion des opérations de contrôleur de domaine|-Création et suppression des contrôleurs de domaine<br />-Analyse le fonctionnement des contrôleurs de domaine<br />-La gestion des services qui sont exécutent sur des contrôleurs de domaine<br />-Sauvegarde et restauration du répertoire|  
|Configuration des paramètres de l’échelle du domaine|-Création de domaine et utilisateur de domaine des stratégies de compte, comme mot de passe, Kerberos et les stratégies de verrouillage de compte<br />-Créer et appliquer la stratégie de groupe au niveau du domaine|  
|Délégation de l’administration de niveau de données|-Créer des unités d’organisation (UO) et la délégation de l’administration<br />-La réparation dans la structure d’unité d’organisation qui propriétaires de l’unité d’organisation n’ont pas de droits d’accès suffisants pour résoudre les problèmes|  
|Gestion des approbations externes|-Établir des relations d’approbation avec les domaines situés en dehors de la forêt|  
  
Autres types de gestion des services, tels que de schéma ou de gestion de topologie de réplication, est responsable du propriétaire de la forêt.  
  
## <a name="domain-owner"></a>Propriétaire du domaine  
Dans un modèle de forêt d’organisation de domaine, les propriétaires de domaine sont responsables de tâches de gestion de service au niveau du domaine. Les propriétaires de domaine ont autorité sur l’ensemble du domaine ainsi qu’un accès à tous les autres domaines dans la forêt. Pour cette raison, les propriétaires de domaine doivent être sélectionnées par le propriétaire de la forêt de personnes de confiance.  
  
Déléguer la gestion de service au niveau du domaine à un propriétaire de domaine, si les conditions suivantes sont remplies:  
  
-   Tous les groupes participant à la forêt approuvent le nouveau propriétaire du domaine et les pratiques de gestion de service du nouveau domaine.  
  
-   Le nouveau propriétaire du domaine approuve le propriétaire de la forêt et tous les autres propriétaires de domaine.  
  
-   Tous les propriétaires de domaine dans la forêt acceptez que le nouveau propriétaire du domaine a pratiques sont égales à ou plus stricte que leurs propres et stratégies de sélection et gestion des administrateurs de service.  
  
-   Tous les propriétaires de domaine dans la forêt acceptez que les contrôleurs de domaine gérés par le nouveau propriétaire du domaine dans le nouveau domaine sont physiquement sécurisés.  
  
Notez que si une forêt propriétaire délégués service au niveau du domaine de gestion pour un propriétaire de domaine, autres groupes peuvent choisissez de ne pas joindre cette forêt si elles n’approuvent pas le propriétaire du domaine.  
  
Tous les propriétaires de domaine doivent prendre en charge que si un de ces conditions modifié à l’avenir, il peut s’avérer nécessaire de déplacer les domaines d’organisation dans un déploiement à forêts multiples.  
  
> [!NOTE]  
> Une autre façon de réduire les risques de sécurité à un domaine Windows Server 2008 Active Directory consiste à utiliser la séparation des rôles administrateur, ce qui nécessite le déploiement d’un contrôleur de domaine en lecture seule (RODC) dans votre infrastructure Active Directory. Un RODC est un nouveau type de contrôleur de domaine dans le système d’exploitation Windows Server 2008 qui héberge les partitions en lecture seule de la base de données Active Directory. Avant la version de Windows Server 2008, les travaux de maintenance de serveur sur un contrôleur de domaine devait être effectuée par un administrateur de domaine. Dans Windows Server 2008, vous pouvez déléguer des autorisations d’administrateur local pour un RODC à n’importe quel utilisateur de domaine sans se voir accorder les droits d’administration pour le domaine ou d’autres contrôleurs de domaine. Cela permet à l’utilisateur délégué pour ouvrir une session sur un RODC et réaliser des tâches de maintenance, telles que la mise à niveau d’un pilote, sur le serveur. Toutefois, cet utilisateur délégué ne peut pas ouvrir une session sur tout autre contrôleur de domaine ou effectuer toutes les tâches d’administration dans le domaine. De cette façon, tout approuvé utilisateur peut être déléguée la possibilité de gérer efficacement le RODC sans compromettre la sécurité du reste du domaine. Pour plus d’informations sur les RODC, voir les services ADDS: Read-Only contrôleurs de domaine ([https://go.microsoft.com/fwlink/?LinkId=106616](https://go.microsoft.com/fwlink/?LinkId=106616)).  
  



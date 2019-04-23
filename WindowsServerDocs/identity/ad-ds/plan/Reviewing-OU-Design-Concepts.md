---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Examen des concepts d’unité d’organisation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f05104466c1cedcfbc8d94060ffa8fbfd9d18033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832170"
---
# <a name="reviewing-ou-design-concepts"></a>Examen des concepts d’unité d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La structure d’unité d’organisation (UO) pour un domaine inclut les éléments suivants :  
  
-   Un diagramme de la hiérarchie d’UO  
  
-   Une liste d’unités d’organisation  
  
-   Pour chaque unité d’organisation :  
  
    -   L’objectif de l’unité d’organisation  
  
    -   Une liste des utilisateurs ou groupes qui ont un contrôle sur l’unité d’organisation ou les objets dans l’unité d’organisation  
  
    -   Le type de contrôle dont disposent les utilisateurs et groupes sur les objets dans l’unité d’organisation  
  
La hiérarchie d’UO n’a pas besoin afin de refléter la hiérarchie département de l’organisation ou le groupe. Unités d’organisation sont créées dans un but spécifique, telles que la délégation d’administration, l’application de stratégie de groupe, ou pour limiter la visibilité des objets.  
  
Vous pouvez concevoir votre structure d’unité d’organisation pour déléguer l’administration à des personnes ou groupes au sein de votre organisation qui nécessitent l’autonomie à gérer leurs propres ressources et les données. Unités d’organisation représentent les limites administratives et vous permettent de contrôler la portée de l’autorité des administrateurs de données.  
  
Par exemple, vous pouvez créer une unité d’organisation appelée ResourceOU et l’utiliser pour stocker tous les comptes d’ordinateurs qui appartiennent au fichier et gérés par un groupe de serveurs d’impression. Puis, vous pouvez configurer la sécurité sur l’unité d’organisation afin que seuls les administrateurs de données dans le groupe aient accès à l’unité d’organisation. Cela empêche les administrateurs de données dans d’autres groupes de falsifier le fichier et les comptes de serveur d’impression.  
  
Vous pouvez affiner davantage votre structure d’unité d’organisation en créant des sous-arborescences d’unités d’organisation à des fins spécifiques, telles que l’application de stratégie de groupe ou pour limiter la visibilité des objets protégés afin que seuls certains utilisateurs puissent les voir. Par exemple, si vous avez besoin appliquer la stratégie de groupe à un groupe sélectionné d’utilisateurs ou des ressources, ajoutez les utilisateurs ou les ressources à une unité d’organisation et ensuite appliquer une stratégie de groupe pour cette unité d’organisation. Vous pouvez également utiliser la hiérarchie d’UO pour activer la délégation du contrôle administratif supplémentaire.  
  
S’il n’existe aucune limite technique pour le nombre de niveaux dans votre structure d’unité d’organisation, pour la gestion de nous vous recommandons de limiter votre structure d’unité d’organisation à une profondeur de pas plus de 10 niveaux. Il n’existe aucune limite au nombre d’unités d’organisation sur chaque niveau technique. Notez que Services de domaine Active Directory (AD DS)-applications activées peuvent avoir des restrictions sur le nombre de caractères utilisé dans le nom unique (autrement dit, le Lightweight Directory Access Protocol (LDAP) chemin d’accès complet à l’objet dans le répertoire) ou sur le Profondeur d’unité d’organisation au sein de la hiérarchie.  
  
La structure d’UO dans AD DS ne vise pas à être visible aux utilisateurs finaux. La structure d’unité d’organisation est un outil d’administration pour les administrateurs de service et pour les administrateurs de données, et il est facile de modifier. Continuez à consulter et mettre à jour de votre conception de structure d’unité d’organisation afin de refléter les modifications dans votre structure d’administration et pour prendre en charge de l’administration basée sur la stratégie.  
  



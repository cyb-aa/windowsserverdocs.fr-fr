---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: Examen des concepts d’unité d’organisation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 6dc2cbb7ddff8725876f8dd4ec2760e828fd4e4c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402517"
---
# <a name="reviewing-ou-design-concepts"></a>Examen des concepts d’unité d’organisation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La structure de l’unité d’organisation d’un domaine comprend les éléments suivants :  
  
-   Diagramme de la hiérarchie d’UO  
  
-   Liste d’unités d’organisation  
  
-   Pour chaque unité d’organisation :  
  
    -   L’objectif de l’unité d’organisation  
  
    -   Liste des utilisateurs ou des groupes qui contrôlent l’unité d’organisation ou les objets de l’unité d’organisation.  
  
    -   Type de contrôle que les utilisateurs et les groupes ont sur les objets de l’unité d’organisation  
  
La hiérarchie d’UO n’a pas besoin de refléter la hiérarchie départementale de l’organisation ou du groupe. Les unités d’organisation sont créées à des fins spécifiques, telles que la délégation de l’administration, l’application de stratégie de groupe ou pour limiter la visibilité des objets.  
  
Vous pouvez concevoir votre structure d’UO pour déléguer l’administration à des personnes ou des groupes de votre organisation qui requièrent l’autonomie pour gérer leurs propres ressources et données. Les unités d’organisation représentent les limites administratives et vous permettent de contrôler l’étendue de l’autorité des administrateurs de données.  
  
Par exemple, vous pouvez créer une unité d’organisation appelée ResourceOU et l’utiliser pour stocker tous les comptes d’ordinateur qui appartiennent au fichier et aux serveurs d’impression gérés par un groupe. Ensuite, vous pouvez configurer la sécurité sur l’unité d’organisation afin que seuls les administrateurs de données du groupe aient accès à l’unité d’organisation. Cela empêche les administrateurs de données dans d’autres groupes de falsifier les comptes de serveur de fichiers et d’impression.  
  
Vous pouvez affiner davantage votre structure d’unité d’organisation en créant des sous-arborescences d’unités d’organisation à des fins spécifiques, telles que l’application d’stratégie de groupe ou pour limiter la visibilité des objets protégés afin que seuls certains utilisateurs puissent les voir. Par exemple, si vous devez appliquer des stratégie de groupe à un groupe sélectionné d’utilisateurs ou de ressources, vous pouvez ajouter ces utilisateurs ou ressources à une unité d’organisation, puis appliquer des stratégie de groupe à cette UO. Vous pouvez également utiliser la hiérarchie d’unités d’organisation pour permettre une délégation supplémentaire du contrôle administratif.  
  
Bien qu’il n’existe aucune limite technique quant au nombre de niveaux dans votre structure d’unité d’organisation, nous vous recommandons de limiter votre structure d’UO à une profondeur inférieure ou égale à 10 niveaux. Il n’existe aucune limite technique quant au nombre d’unités d’organisation à chaque niveau. Notez que les applications prenant en charge les Active Directory Domain Services (AD DS) peuvent avoir des restrictions sur le nombre de caractères utilisés dans le nom unique (autrement dit, le chemin d’accès LDAP (Lightweight Directory Access Protocol) complet à l’objet dans le répertoire) ou sur le Profondeur de l’unité d’organisation dans la hiérarchie.  
  
La structure d’unité d’organisation dans AD DS n’est pas destinée à être visible par les utilisateurs finaux. La structure d’UO est un outil d’administration pour les administrateurs de service et pour les administrateurs de données, et il est facile à modifier. Passez en revue et mettez à jour la conception de votre structure d’UO pour refléter les modifications apportées à votre structure administrative et pour prendre en charge l’administration basée sur des stratégies.  
  



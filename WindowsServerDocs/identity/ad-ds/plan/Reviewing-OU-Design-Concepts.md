---
ms.assetid: 41b56704-c6f9-4d29-af97-62123e300565
title: "Examen des Concepts de conception d’unité d’organisation"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e832d068a4d03316853d8b59e3f2ac4a6ebc816
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="reviewing-ou-design-concepts"></a>Examen des Concepts de conception d’unité d’organisation

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

La structure d’unité d’organisation (UO) pour un domaine inclut les éléments suivants:  
  
-   Un diagramme de la hiérarchie de l’unité d’organisation  
  
-   Une liste d’unités d’organisation  
  
-   Pour chaque unité d’organisation:  
  
    -   L’objectif de l’unité d’organisation  
  
    -   Une liste des utilisateurs ou groupes qui possèdent le contrôle sur l’unité d’organisation ou les objets dans l’unité d’organisation  
  
    -   Le type de contrôle dont disposent les utilisateurs et les groupes sur les objets dans l’unité d’organisation  
  
La hiérarchie d’UO n’a pas besoin afin de refléter la hiérarchie de service de l’organisation ou le groupe. Unités d’organisation sont créées dans un but spécifique, par exemple, la délégation de l’administration, l’application de stratégie de groupe, ou pour limiter la visibilité des objets.  
  
Vous pouvez concevoir votre structure d’unité d’organisation pour déléguer l’administration à des personnes ou des groupes au sein de votre organisation qui nécessitent l’autonomie pour gérer leurs propres ressources et des données. Unités d’organisation représentent les limites d’administration et vous permettent de contrôler l’étendue de l’autorité des administrateurs de données.  
  
Par exemple, vous pouvez créer une unité d’organisation appelée ResourceOU et l’utiliser pour stocker tous les comptes d’ordinateurs qui appartiennent au fichier et gérés par un groupe de serveurs d’impression. Puis, vous pouvez configurer la sécurité sur l’unité d’organisation afin que seuls les administrateurs de données dans le groupe ont accès à l’unité d’organisation. Cela empêche les administrateurs de données dans d’autres groupes de falsifier les fichiers et les comptes de serveur d’impression.  
  
Vous pouvez affiner votre structure d’unité d’organisation en créant des sous-arbres d’unités d’organisation à des fins spécifiques, tels que l’application de stratégie de groupe ou pour limiter la visibilité des objets protégés afin que seuls certains utilisateurs peuvent les voir. Par exemple, si vous avez besoin appliquer la stratégie de groupe à un groupe d’utilisateurs ou des ressources, vous pouvez ajouter les utilisateurs ou les ressources à une unité d’organisation et ensuite appliquer stratégie de groupe à cette unité d’organisation. Vous pouvez également utiliser la hiérarchie d’UO pour activer la délégation de contrôle d’administration supplémentaire.  
  
Il n’existe aucune limite au nombre de niveaux de votre structure d’unité d’organisation technique, pour la gestion de nous vous conseillons de limiter votre structure d’unité d’organisation à une profondeur de pas plus de 10 niveaux. Il n’existe aucune limite au nombre d’unités d’organisation sur chaque niveau technique. Notez que Services de domaine ActiveDirectory (ADDS)-applications compatibles peuvent avoir des restrictions sur le nombre de caractères utilisé dans le nom unique (autrement dit, le répertoire accès protocole LDAP (Lightweight) chemin d’accès complet à l’objet dans le répertoire) ou la profondeur de l’unité d’organisation au sein de la hiérarchie.  
  
La structure d’unité d’organisation dans les services ADDS ne vise pas à être visible pour les utilisateurs finaux. La structure d’unité d’organisation est un outil d’administration pour les administrateurs de service et les administrateurs de données, et il est facile de le modifier. Continuez à examiner et mettre à jour de votre conception de structure d’unité d’organisation afin de refléter les modifications apportées à votre structure administrative et pour prendre en charge l’administration basée sur la stratégie.  
  



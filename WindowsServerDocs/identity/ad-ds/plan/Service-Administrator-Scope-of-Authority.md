---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Étendue de l’autorité de l’administrateur de service
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b5bf2fb3b06a47d730b9dd124b2b66a0a4c9c691
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864850"
---
# <a name="service-administrator-scope-of-authority"></a>Étendue de l’autorité de l’administrateur de service

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous choisissez de participer à une forêt Active Directory, vous devez approuver le propriétaire de la forêt et les administrateurs de service. Les propriétaires de la forêt sont responsables de sélection et la gestion des administrateurs de service ; Par conséquent, lorsque vous faites confiance à un propriétaire de la forêt, vous également faire confiance aux administrateurs de service qui gère le propriétaire de la forêt. Ces administrateurs de service ont accès à toutes les ressources dans la forêt. Avant de décider de participer à une forêt, il est important de comprendre que le propriétaire de la forêt et les administrateurs de service ont un accès complet à vos données. Vous ne pouvez pas empêcher cet accès.  
  
Tous les administrateurs de service dans une forêt ont un contrôle total sur toutes les données et services sur tous les ordinateurs dans la forêt. Les administrateurs de service ont la possibilité d’effectuer les opérations suivantes :  
  
-   Corrigez les erreurs sur les listes de contrôle d’accès (ACL) d’objets. Cela permet à l’administrateur de service lire, modifier ou supprimer des objets, quel que soit les ACL sont définies sur ces objets.  
  
-   Modifier le logiciel système sur un contrôleur de domaine à contourner les vérifications de sécurité normales. Cela permet à l’administrateur de service afficher ou manipuler n’importe quel objet dans le domaine, quel que soit l’ACL sur l’objet.  
  
-   Utilisez la stratégie de sécurité des groupes restreints pour accorder pour n’importe quel utilisateur ou groupe l’accès administratif à n’importe quel ordinateur joint au domaine. De cette façon, les administrateurs de service peuvent prendre le contrôle de n’importe quel ordinateur joint au domaine, quel que soit les intentions du propriétaire de l’ordinateur.  
  
-   Réinitialiser les mots de passe ou modifier les appartenances aux groupes pour les utilisateurs.  
  
-   Accéder à d’autres domaines dans la forêt en modifiant le logiciel du système sur un contrôleur de domaine. Les administrateurs de service peuvent affecter le fonctionnement de tout domaine inclus dans la forêt, vue ou manipuler les données de configuration de forêt, afficher ou manipuler les données stockées dans n’importe quel domaine et afficher ou manipuler les données stockées sur n’importe quel ordinateur joint à la forêt.  
  
Pour cette raison, il s’agit de groupes qui stockent des données dans des unités d’organisation (UO) dans la forêt et que les ordinateurs de jointure à une forêt doivent approuver les administrateurs de service. Pour un groupe à joindre une forêt, il doit choisir d’approuver tous les administrateurs de service dans la forêt. Cela implique de s’assurer que :  
  
-   Le propriétaire de la forêt peut être approuvé pour agir dans l’intérêt du groupe et n’a pas de raison d’agir à des fins malveillantes sur le groupe.  
  
-   Le propriétaire de la forêt limite en conséquence l’accès physique aux contrôleurs de domaine. Contrôleurs de domaine dans une forêt ne peut pas être isolés les uns des autres. Il est possible pour un attaquant possédant un accès physique à un seul contrôleur de domaine pour apporter des modifications hors connexion à la base de données d’annuaire et, en procédant ainsi, interférer avec l’opération de n’importe quel domaine de la forêt, afficher ou manipuler les données stockées n’importe où dans la forêt et afficher ou manipuler les données stockées sur n’importe quel ordinateur joint à la forêt. Pour cette raison, l’accès physique aux contrôleurs de domaine doit être limité aux personnes dignes de confiance.  
  
-   Comprendre et accepter les risques potentiels qui approuvé les administrateurs de service pouvant être convertis en compromettre la sécurité du système.  
  
Certains groupes peuvent déterminer que les avantages de collaboration et des coûts de participer à une infrastructure partagée compensent les risques que les administrateurs de service utilisera à mauvais escient ou seront converties en outrepassent leur autorité. Ces groupes peuvent partager une forêt et utiliser des unités d’organisation pour déléguer l’autorité. Autres groupes peuvent cependant pas accepter ce risque, car les conséquences d’une compromission de la sécurité sont trop graves. Ces groupes nécessitent des forêts distinctes.  
  



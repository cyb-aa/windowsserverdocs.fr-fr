---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: "Administrateur de service étendue de l’autorité"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ba682067b9c7a7b4bf583482abe6470b60b25450
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="service-administrator-scope-of-authority"></a>Administrateur de service étendue de l’autorité

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Si vous choisissez de participer à une forêt ActiveDirectory, vous devez approuver le propriétaire de la forêt et les administrateurs de service. Les propriétaires de forêt sont chargés pour la sélection et la gestion des administrateurs de service; par conséquent, lorsque vous faites confiance à un propriétaire de la forêt, vous avez également approuver les administrateurs de service qui gère le propriétaire de la forêt. Ces administrateurs de service ont accès à toutes les ressources de la forêt. Avant de décider de participer à une forêt, il est important de comprendre que le propriétaire de la forêt et les administrateurs de service ont un accès complet à vos données. Vous ne pouvez pas empêcher cet accès.  
  
Tous les administrateurs de service dans une forêt ont un contrôle total sur tous les services et les données sur tous les ordinateurs dans la forêt. Les administrateurs de service ont la possibilité d’effectuer les opérations suivantes:  
  
-   Corriger les erreurs sur les listes de contrôle d’accès (ACL) des objets. Cela permet à l’administrateur de service lire, modifier ou supprimer des objets, quelle que soit l’ACL sont définis sur ces objets.  
  
-   Modifier le logiciel du système sur un contrôleur de domaine de contourner les contrôles de sécurité normale. Cela permet à l’administrateur de service afficher ou manipuler de n’importe quel objet dans le domaine, quelle que soit la liste ACL sur l’objet.  
  
-   Utilisez la stratégie de sécurité groupes restreints pour accorder n’importe quel utilisateur ou un groupe d’administration de l’accès à n’importe quel ordinateur joint au domaine. De cette façon, les administrateurs de service peuvent prendre le contrôle de n’importe quel ordinateur joint au domaine, quelle que soit les intentions de propriétaire de l’ordinateur.  
  
-   Réinitialiser les mots de passe ou modifier les membres du groupe pour les utilisateurs.  
  
-   Accéder à d’autres domaines dans la forêt en modifiant le logiciel du système sur un contrôleur de domaine. Les administrateurs de service affectent le fonctionnement de n’importe quel domaine de la forêt, afficher ou manipuler les données de configuration de forêt, afficher ou manipuler les données stockées dans n’importe quel domaine et pour afficher ou manipuler les données stockées sur n’importe quel ordinateur joint à la forêt.  
  
Pour cette raison, les groupes qui stockent les données dans des unités d’organisation (UO) dans la forêt et que les ordinateurs de jointure à une forêt doivent approuver les administrateurs de service. Pour un groupe à joindre une forêt, elle doit choisir d’approuver tous les administrateurs de service dans la forêt. Cela implique de veiller à ce que:  
  
-   Le propriétaire de la forêt peut être approuvé pour agir dans l’intérêt du groupe et ne possède pas de raison d’agir à des fins malveillantes sur le groupe.  
  
-   Le propriétaire de la forêt limite en conséquence l’accès physique aux contrôleurs de domaine. Contrôleurs de domaine dans une forêt ne peut pas être isolées les unes des autres. Il est possible qu’une personne malveillante qui dispose d’un accès physique à un contrôleur de domaine unique pour apporter des modifications hors connexion pour la base de données active et, en procédant ainsi, interférer avec l’opération de n’importe quel domaine de la forêt, afficher ou manipuler les données stockées n’importe où dans la forêt et afficher ou manipuler les données stockées sur n’importe quel ordinateur joint à la forêt. Pour cette raison, l’accès physique aux contrôleurs de domaine doit être limité aux personnes dignes de confiance.  
  
-   Vous comprenez et acceptez les risques potentiels qui approuvé administrateurs de service peuvent être convertis en compromettre la sécurité du système.  
  
Certains groupes peuvent déterminer que les avantages de collaboration et des coûts de participer à une infrastructure partagée l’emportent sur les risques que les administrateurs de service seront utilisation abusive ou seront converties en mauvais usage de leur autorité. Ces groupes peuvent partager une forêt et utiliser des unités d’organisation pour déléguer l’autorité. Toutefois, autres groupes ne peuvent pas accepter ce risque, car les conséquences d’une compromission de sécurité sont trop graves. Ces groupes nécessitent des forêts distinctes.  
  



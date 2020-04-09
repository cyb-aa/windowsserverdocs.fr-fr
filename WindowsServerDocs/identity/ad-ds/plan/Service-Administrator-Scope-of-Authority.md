---
ms.assetid: da7b6dcf-53ec-4394-88c0-c087d92f3893
title: Étendue de l’autorité de l’administrateur de service
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3a891ade46fdee1dffc35df31a11c6d138e71e8a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821892"
---
# <a name="service-administrator-scope-of-authority"></a>Étendue de l’autorité de l’administrateur de service

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Si vous choisissez de participer à une forêt Active Directory, vous devez faire confiance au propriétaire de la forêt et aux administrateurs de service. Les propriétaires de la forêt sont responsables de la sélection et de la gestion des administrateurs de service. par conséquent, lorsque vous faites confiance à un propriétaire de forêt, vous faites également confiance aux administrateurs de service gérés par le propriétaire de la forêt. Ces administrateurs de service ont accès à toutes les ressources de la forêt. Avant de prendre la décision de participer à une forêt, il est important de comprendre que le propriétaire de la forêt et les administrateurs de service auront un accès complet à vos données. Vous ne pouvez pas empêcher cet accès.  
  
Tous les administrateurs de service d’une forêt ont un contrôle total sur l’ensemble des données et des services sur tous les ordinateurs de la forêt. Les administrateurs de service ont la possibilité d’effectuer les opérations suivantes :  
  
-   Corriger les erreurs sur les listes de contrôle d’accès (ACL) d’objets. Cela permet à l’administrateur de service de lire, modifier ou supprimer des objets indépendamment des listes de contrôle d’accès définies sur ces objets.  
  
-   Modifiez le logiciel système sur un contrôleur de domaine pour contourner les vérifications de sécurité normales. Cela permet à l’administrateur de service d’afficher ou de manipuler n’importe quel objet dans le domaine, quelle que soit la liste de contrôle d’accès de l’objet.  
  
-   Utilisez la stratégie de sécurité groupes restreints pour accorder à un utilisateur ou à un groupe un accès administratif à tout ordinateur joint au domaine. De cette façon, les administrateurs de service peuvent obtenir le contrôle de tout ordinateur joint au domaine, quelles que soient les intentions du propriétaire de l’ordinateur.  
  
-   Réinitialiser les mots de passe ou modifier l’appartenance aux groupes pour les utilisateurs.  
  
-   Accédez à d’autres domaines de la forêt en modifiant le logiciel système sur un contrôleur de domaine. Les administrateurs de services peuvent affecter le fonctionnement de n’importe quel domaine de la forêt, afficher ou manipuler les données de configuration de la forêt, afficher ou manipuler les données stockées dans n’importe quel domaine, et afficher ou manipuler les données stockées sur tout ordinateur joint à la forêt.  
  
Pour cette raison, les groupes qui stockent des données dans des unités d’organisation (UO) de la forêt et qui joignent des ordinateurs à une forêt doivent faire confiance aux administrateurs de service. Pour qu’un groupe rejoigne une forêt, il doit choisir d’approuver tous les administrateurs de service dans la forêt. Cela implique de s’assurer que :  
  
-   Le propriétaire de la forêt peut être approuvé pour agir dans l’intérêt du groupe et n’a pas de raison d’agir de manière malveillante contre le groupe.  
  
-   Le propriétaire de la forêt restreint de manière appropriée l’accès physique aux contrôleurs de domaine. Les contrôleurs de domaine d’une forêt ne peuvent pas être isolés les uns des autres. Il est possible pour une personne malveillante ayant un accès physique à un contrôleur de domaine unique d’apporter des modifications hors connexion à la base de données d’annuaire et, en procédant ainsi, d’interférer avec le fonctionnement de n’importe quel domaine de la forêt, d’afficher ou de manipuler des données stockées n’importe où dans la forêt, et d’afficher ou de manipuler des données stockées sur un ordinateur Pour cette raison, l’accès physique aux contrôleurs de domaine doit être limité au personnel de confiance.  
  
-   Vous comprenez et acceptez le risque potentiel que les administrateurs de services approuvés puissent être forcés à compromettre la sécurité du système.  
  
Certains groupes peuvent déterminer que les avantages collaboratifs et économiques associés à la participation à une infrastructure partagée compensent les risques que les administrateurs de service pourraient abuser ou seront contraints d’utiliser leur autorité de manière incorrecte. Ces groupes peuvent partager une forêt et utiliser des unités d’organisation pour déléguer l’autorité. Toutefois, d’autres groupes peuvent ne pas accepter ce risque, car les conséquences d’un compromis en matière de sécurité sont trop graves. Ces groupes requièrent des forêts distinctes.  
  



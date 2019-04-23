---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomie et. Isolation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 765a25d3d1ffdb4df473e1fb5bb65e532934aca9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867570"
---
# <a name="autonomy-vs-isolation"></a>Autonomie et. Isolation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez concevoir votre structure Active Directory logique pour obtenir de le des manières suivantes :  
  
-   **Autonomie**. Implique le contrôle indépendant mais non exclusif d’une ressource. Lorsque vous obtenez l’autonomie, les administrateurs ont l’autorité nécessaire pour gérer des ressources de manière indépendante ; Toutefois, les administrateurs disposant d’une autorité supérieure existent qui également de contrôler ces ressources et de prendre le contrôle immédiatement si nécessaire. Vous pouvez concevoir votre structure Active Directory logique pour obtenir les types suivants d’autonomie :  
  
    -   **Autonomie de service**. Ce type d’autonomie implique le contrôle tout ou partie de la gestion des services.  
  
    -   **Autonomie des données**. Ce type d’autonomie implique le contrôle tout ou partie des données stockées dans le répertoire ou sur les ordinateurs membres rattachés à l’annuaire.  
  
-   **Isolation**. Implique le contrôle indépendant et exclusif d’une ressource. Lorsque vous isoler, les administrateurs ont l’autorité nécessaire pour gérer une ressource de manière indépendante et aucun autre administrateur ne peut prendre le contrôle de la ressource de suite. Vous pouvez concevoir votre structure Active Directory logique pour obtenir les types d’isolation suivants :  
  
    -   **Isolation de service**. Empêche les administrateurs (autres que les administrateurs qui sont spécifiquement désignés pour contrôler la gestion des services) de contrôler ou interférer avec la gestion des services.  
  
    -   **Isolation des données**. Empêche les administrateurs (autres que les administrateurs qui sont désignés spécifiquement pour les données de contrôle ou une vue) de contrôler ou de consulter un sous-ensemble de données dans le répertoire ou sur les ordinateurs membres rattachés à l’annuaire.  
  
Les administrateurs qui ont besoin d’autonomie uniquement acceptent qu’autres administrateurs disposant d’autorité administrative supérieure ou égale contrôlent égale ou supérieure sur la gestion des services ou des données. Les administrateurs qui ont besoin d’isolation ont un contrôle exclusif sur la gestion de service ou les données. Création d’une conception pour obtenir l’autonomie est généralement moins onéreuse que la création d’une conception à isoler.  
  
Dans les Services de domaine Active Directory (AD DS), les administrateurs peuvent déléguer à la fois service et l’administration des données pour atteindre l’autonomie ou isolation entre les organisations. La combinaison de gestion des services, des exigences de gestion, d’autonomie et d’isolation des données d’une organisation un impact sur les conteneurs Active Directory qui sont utilisés pour déléguer l’administration.  
  
## <a name="isolation-and-autonomy-requirements"></a>Exigences d’isolation et d’autonomie  
Le nombre de forêts dont vous avez besoin pour déployer est basé sur les exigences d’autonomie et d’isolation de chaque groupe au sein de votre organisation. Pour identifier vos besoins de conception de forêt, vous devez identifier les exigences d’autonomie et d’isolation pour tous les groupes dans votre organisation. Plus précisément, vous devez identifier la nécessité d’isolation des données, données autonomie, l’isolation de service et l’autonomie des services. Vous devez également identifier les zones d’une connectivité limitée dans votre organisation.  
  
### <a name="data-isolation"></a>Isolation des données  
Isolation des données implique le contrôle exclusif des données par le groupe ou organisation qui possède les données. Il est important de noter que les administrateurs de service ont la possibilité de prendre le contrôle d’une ressource en dehors des administrateurs de données. Et les administrateurs de données n’ont pas la possibilité d’empêcher les administrateurs de service à partir de l’accès aux ressources dont ils ont. Par conséquent, vous ne pouvez pas obtenir isolation des données lorsqu’un autre groupe au sein de l’organisation est responsable de l’administration des services. Si un groupe nécessite l’isolation des données, ce groupe doit également assumer la responsabilité pour l’administration de service.  
  
Étant donné que les données stockées dans AD DS et sur les ordinateurs joints à AD DS ne peut pas être isolés des administrateurs de service, la seule façon pour un groupe au sein d’une organisation à isoler la totalité des données consiste à créer une forêt distincte pour ces données. Les organisations qui les conséquences d’une attaque par un logiciel malveillant ou par un administrateur de service forcée sont substantielles peuvent choisir de créer une forêt distincte pour isoler des données. Exigences légales créent généralement un besoin de ce type d’isolation des données. Exemple :  
  
-   Une institution financière est requis par la loi pour limiter l’accès aux données qui appartiennent aux clients dans une juridiction donnée aux utilisateurs, ordinateurs et les administrateurs situés dans cette juridiction. Bien que l’établissement approuve les administrateurs de service qui fonctionnent en dehors de la zone protégée, si la limitation de l’accès est violée, il se peut que l’établissement ne seront plus en mesure d’effectuer des activités dans cette juridiction. Par conséquent, l’institution financière doit isoler des données auprès des administrateurs de service en dehors de cette compétence. Notez que le chiffrement n’est pas toujours une alternative à cette solution. Chiffrement ne peut pas protéger les données à partir des administrateurs de service.  
  
-   Un sous-traitant pour la défense est requis par la loi pour limiter l’accès aux données de projet pour un ensemble spécifié d’utilisateurs. Bien que le fournisseur approuve les administrateurs de service qui contrôlent les systèmes informatiques liés à d’autres projets, une violation de cette limitation d’accès entraîne l’entrepreneur perdre d’entreprise.  
  
    > [!NOTE]  
    > Si vous avez une exigence d’isolation de données, vous devez décider si vous devez isoler vos données à partir des administrateurs de service ou à partir des administrateurs de données et les utilisateurs ordinaires. Si vos besoins d’isolation est basé sur l’isolation des administrateurs de données et les utilisateurs ordinaires, vous pouvez utiliser des listes de contrôle d’accès (ACL) pour isoler les données. Dans le cadre de ce processus de conception, isolation des administrateurs de données et les utilisateurs ordinaires n'est pas considéré comme une exigence d’isolation de données.  
  
### <a name="data-autonomy"></a>Autonomie des données  
Autonomie des données implique la possibilité d’un groupe ou d’organisation pour gérer ses propres données, y compris les décisions d’administration sur les données et effectuer les tâches d’administration requises sans la nécessité d’approbation à partir d’une autre autorité.  
  
Autonomie des données n’empêche pas les administrateurs de service dans la forêt à partir de l’accès aux données. Par exemple, un groupe de recherche d’une grande entreprise peut souhaitez être en mesure de gérer leurs données spécifiques au projet eux-mêmes, mais n’est pas nécessaire sécuriser les données à partir d’autres administrateurs dans la forêt.  
  
### <a name="service-isolation"></a>Isolation de service  
Isolation de service implique le contrôle exclusif de l’infrastructure Active Directory. Groupes qui requièrent l’isolation de service nécessitent qu’aucun administrateur en dehors du groupe ne peut interférer avec l’opération du service d’annuaire.  
  
Les exigences opérationnelles ou légales créent généralement un besoin pour l’isolation de service. Exemple :  
  
-   Une société de fabrication dispose d’une application critique qui contrôle l’équipement en usine. Interruptions dans le service sur d’autres parties du réseau de l’organisation ne peut pas être autorisées à interférer avec le fonctionnement de l’usine.  
  
-   Une société d’hébergement fournit un service à plusieurs clients. Chaque client nécessite l’isolation de service afin que toute interruption de service qui affecte un client n’affecte pas les autres clients.  
  
### <a name="service-autonomy"></a>Autonomie des services  
L’autonomie des services implique la possibilité de gérer l’infrastructure sans une condition requise pour le contrôle exclusif ; par exemple, lorsqu’un groupe souhaite apporter des modifications à l’infrastructure (par exemple, ajout ou suppression de domaines, modification de l’espace de noms du système DNS (Domain Name) ou la modification du schéma) sans l’approbation du propriétaire de la forêt.  
  
L’autonomie des services pouvant être nécessaire au sein d’une organisation pour un groupe qui souhaite être en mesure de contrôler le niveau de service des services AD DS (en ajoutant et supprimant des contrôleurs de domaine, en fonction des besoins) ou pour un groupe qui doit être en mesure d’installer des applications d’annuaire qui exiger des extensions de schéma.  
  
## <a name="limited-connectivity"></a>Connectivité limitée  
Si un groupe au sein de votre organisation est propriétaire réseaux séparés par des appareils afin de restreindre ou limitent la connectivité entre réseaux (tels que les pare-feux et périphériques de traduction d’adresses réseau (NAT)), cela peut affecter votre conception de forêt. Lorsque vous identifiez vos besoins de conception de forêt, prenez soin de noter les emplacements où vous avez limité connectivité réseau. Ces informations sont nécessaires pour pouvoir prendre des décisions concernant la conception de forêt.  
  



---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomie et Isolation
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 19613b209399c61747af6a2d1fbe243dbcb92225
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="autonomy-vs-isolation"></a>Autonomie et Isolation

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Vous pouvez concevoir votre structure logique ActiveDirectory pour atteindre une des opérations suivantes:  
  
-   **Autonomie**. Implique le contrôle indépendant mais non exclusif d’une ressource. Lorsque vous assurer l’autonomie, les administrateurs ont l’autorité pour gérer les ressources de manière indépendante; toutefois, les administrateurs avec une autorité supérieure existent qui également de contrôler ces ressources et peuvent prendre le contrôle immédiatement si nécessaire. Vous pouvez concevoir votre structure logique ActiveDirectory pour obtenir les types d’autonomie suivants:  
  
    -   **Autonomie de service**. Ce type d’autonomie implique le contrôle tout ou partie de la gestion des services.  
  
    -   **Autonomie des données**. Ce type d’autonomie implique le contrôle tout ou partie des données stockées dans le répertoire ou sur les ordinateurs membres rattachés à l’annuaire.  
  
-   **Isolation**. Implique le contrôle indépendant et exclusif d’une ressource. Lorsque vous obtenez d’isolation, les administrateurs ont l’autorisation de gérer une ressource indépendamment et aucun autre administrateur ne peut prendre le contrôle de la ressource absent. Vous pouvez concevoir votre structure logique ActiveDirectory pour obtenir les types d’isolation suivantes:  
  
    -   **Service d’isolation**. Empêche les administrateurs (autres que les administrateurs qui sont spécifiquement désignés pour contrôler la gestion des services) à partir de contrôler ou interférer avec la gestion des services.  
  
    -   **Isolation des données**. Empêche les administrateurs (autres que les administrateurs qui sont spécifiquement désignés pour un contrôle ou un affichage des données) de contrôle ou d’afficher un sous-ensemble des données dans le répertoire ou sur les ordinateurs membres rattachés à l’annuaire.  
  
Les administrateurs qui nécessitent une autonomie uniquement acceptent que les autres administrateurs ayant une autorité administrative supérieure ou égale ont contrôle égale ou supérieure sur la gestion des services ou des données. Les administrateurs qui ont besoin d’isolation ont un contrôle exclusif sur la gestion des services ou des données. Création d’une conception pour assurer l’autonomie est généralement moins cher que la création d’une conception assurant l’isolation.  
  
Dans les Services de domaine ActiveDirectory (ADDS), les administrateurs peuvent déléguer administration des services et administration des données pour effectuer un autonomie ou isolation entre organisations. La combinaison de gestion des services, des exigences de gestion, d’autonomie et isolation des données d’une organisation un impact sur les conteneurs ActiveDirectory qui sont utilisés pour déléguer l’administration.  
  
## <a name="isolation-and-autonomy-requirements"></a>Exigences en matière d’isolement et autonomie  
Le nombre de forêts dont vous avez besoin pour déployer est basé sur les exigences d’autonomie et isolation de chaque groupe au sein de votre organisation. Pour identifier vos besoins en matière de conception de forêt, vous devez identifier les exigences d’autonomie et d’isolation pour tous les groupes de votre organisation. Plus précisément, vous devez identifier la nécessité d’isolation des données, autonomie des données, l’isolation du service et autonomie du service. Vous devez également identifier les zones d’une connectivité limitée dans votre organisation.  
  
### <a name="data-isolation"></a>Isolation des données  
Isolation des données implique un contrôle exclusif sur les données par le groupe ou l’organisation qui est propriétaire des données. Il est important de noter que les administrateurs de service ont la possibilité de prendre le contrôle d’une ressource d’aux administrateurs de données. Et les administrateurs de données n’ont pas la possibilité d’empêcher les administrateurs de service d’accéder aux ressources dont ils ont. Par conséquent, vous ne pouvez pas obtenir d’isolation des données lorsqu’un autre groupe au sein de l’organisation est responsable de l’administration des services. Si un groupe requiert l’isolation des données, ce groupe doit supposer également responsable de l’administration des services.  
  
Étant donné que les données stockées dans ADDS et sur les ordinateurs joints au domaine ActiveDirectory ne peut pas être isolés des administrateurs de service, la seule façon pour un groupe au sein d’une organisation d’isolation complète des données consiste à créer une forêt distincte pour que ces données. Les organisations pour lesquels les conséquences d’une attaque par des logiciels malveillants ou par un administrateur de service forcée sont importantes peuvent choisir de créer une forêt distincte pour atteindre l’isolation des données. En général, les exigences légales créent nécessaire pour ce type d’isolation des données. Par exemple:  
  
-   Une institution financière est requis par la loi pour limiter l’accès aux données qui appartiennent à des clients dans un lieu particulier pour les utilisateurs, les ordinateurs et les administrateurs situés à cette juridiction. Bien que l’établissement des approbations administrateurs de service qui fonctionnent en dehors de la zone protégée, si la limitation de l’accès est enfreinte, l’établissement ne seront plus en mesure de travailler dans cette juridiction. Par conséquent, l’établissement financier doit isoler des données à partir des administrateurs de service en dehors de cette juridiction. Notez que le chiffrement n’est pas toujours une alternative à cette solution. Le chiffrement ne peut pas protéger les données à partir des administrateurs de service.  
  
-   Un prestataire de défense est requis par la loi pour limiter l’accès aux données de projet à un ensemble spécifique d’utilisateurs. Bien que le fournisseur approuve les administrateurs de service qui contrôlent des systèmes informatiques liés à d’autres projets, une violation de cette limitation accès entraîne le titulaire de perdre des entreprises.  
  
    > [!NOTE]  
    > Si vous avez une exigence d’isolation des données, vous devez décider si vous devez isoler vos données à partir d’administrateurs de service ou d’utilisateurs ordinaires et les administrateurs de données. Si vos besoins d’isolation est basé sur l’isolation des administrateurs de données et des utilisateurs ordinaires, vous pouvez utiliser des listes de contrôle d’accès (ACL) pour isoler les données. Dans le cadre de ce processus de conception, l’isolation des administrateurs de données et des utilisateurs ordinaires n'est pas considéré comme une exigence d’isolation des données.  
  
### <a name="data-autonomy"></a>Autonomie des données  
Autonomie des données implique la possibilité de gérer ses propres données, y compris les décisions d’administration sur les données et l’exécution des tâches d’administration requis sans avoir besoin d’approbation à partir d’une autre autorité d’un groupe ou une organisation.  
  
Autonomie des données n’empêche pas les administrateurs de service dans la forêt d’accéder aux données. Par exemple, un groupe de recherche au sein d’une grande organisation peut voulez être en mesure de gérer leurs données spécifiques au projet eux-mêmes, mais n’avez pas besoin de sécuriser les données à partir d’autres administrateurs dans la forêt.  
  
### <a name="service-isolation"></a>Isolation des services  
Isolation des services implique un contrôle exclusif de l’infrastructure ActiveDirectory. Groupes qui nécessitent l’isolation du service nécessitent qu’aucun administrateur en dehors du groupe ne peut interférer avec le fonctionnement du service d’annuaire.  
  
En général, les exigences opérationnelles ou légales créent nécessaire pour l’isolation du service. Par exemple:  
  
-   Une société de fabrication dispose d’une application critique qui contrôle l’équipement en usine. Interruptions dans le service sur d’autres parties du réseau de l’organisation ne peut pas être autorisées à interférer avec le fonctionnement de l’usine.  
  
-   Une société d’hébergement fournit un service à plusieurs clients. Chaque client nécessite l’isolation du service afin que toute interruption de service qui affecte un client n’affecte pas les autres clients.  
  
### <a name="service-autonomy"></a>Autonomie du service  
Autonomie du service implique la possibilité de gérer l’infrastructure sans une condition requise pour un contrôle exclusif; par exemple, lorsqu’un groupe souhaite apporter des modifications à l’infrastructure (par exemple, ajout ou suppression de domaines, modification de l’espace de noms système DNS (Domain Name) ou la modification du schéma) sans l’approbation du propriétaire de la forêt.  
  
Autonomie du service peut être nécessaire au sein d’une organisation pour un groupe qui souhaite pouvoir contrôler le niveau de service des services ADDS (en ajoutant ou supprimant des contrôleurs de domaine, si nécessaire), ou pour un groupe qui doit être en mesure d’installer des applications d’annuaire qui nécessitent des extensions de schéma.  
  
## <a name="limited-connectivity"></a>Connectivité limitée  
Si un groupe au sein de votre organisation possède des réseaux qui sont séparés par des périphériques afin de restreindre ou de limiter la connectivité entre les réseaux (tels que les pare-feu et périphériques de traduction d’adresses réseau (NAT)), ce qui peut affecter votre conception de forêt. Lorsque vous identifiez vos besoins en matière de conception de forêt, veillez à noter les emplacements où vous avez réseau une connectivité limitée. Ces informations sont requises pour vous permettre de prendre des décisions concernant la conception de forêt.  
  



---
ms.assetid: ef63d40c-a262-4a18-938d-b95c10680c0b
title: Autonomie et Isolation
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c3430ae9320ed2d39768d91f768adb3f9ab1c716
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402643"
---
# <a name="autonomy-vs-isolation"></a>Autonomie et Isolation

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Vous pouvez concevoir votre structure logique Active Directory pour obtenir l’un des éléments suivants :  
  
-   **Autonomie**. Implique un contrôle indépendant mais non exclusif d’une ressource. Lorsque vous atteignez l’autonomie, les administrateurs ont l’autorité nécessaire pour gérer les ressources de manière indépendante. Toutefois, les administrateurs disposant d’une autorité plus importante existent qui ont également le contrôle sur ces ressources et peuvent prendre le contrôle si nécessaire. Vous pouvez concevoir votre structure logique Active Directory pour obtenir les types d’autonomie suivants :  
  
    -   **Autonomie du service**. Ce type d’autonomie implique le contrôle de tout ou partie de la gestion des services.  
  
    -   **Autonomie des données**. Ce type d’autonomie implique le contrôle de tout ou partie des données stockées dans le répertoire ou sur les ordinateurs membres joints à l’annuaire.  
  
-   **Isolation**. Implique le contrôle indépendant et exclusif d’une ressource. Lorsque vous atteignez l’isolation, les administrateurs ont l’autorité pour gérer une ressource indépendamment, et aucun autre administrateur ne peut retirer le contrôle de la ressource. Vous pouvez concevoir votre structure logique Active Directory pour obtenir les types d’isolation suivants :  
  
    -   **Isolation du service**. Empêche les administrateurs (autres que ceux qui sont spécifiquement désignés pour le contrôle de la gestion des services) de contrôler ou d’interférer avec la gestion des services.  
  
    -   **Isolation des données**. Empêche les administrateurs (autres que ceux qui sont spécifiquement désignés de contrôler ou d’afficher des données) de contrôler ou d’afficher un sous-ensemble de données dans le répertoire ou sur des ordinateurs membres joints à l’annuaire.  
  
Les administrateurs qui nécessitent uniquement l’autonomie acceptent que les autres administrateurs ayant une autorité administrative égale ou supérieure disposent d’un contrôle égal ou supérieur sur le service ou la gestion des données. Les administrateurs qui ont besoin de l’isolation disposent d’un contrôle exclusif sur la gestion des services ou des données. La création d’une conception pour garantir l’autonomie est généralement moins coûteuse que la création d’une conception pour obtenir une isolation.  
  
Dans Active Directory Domain Services (AD DS), les administrateurs peuvent déléguer l’administration des services et l’administration des données pour assurer l’autonomie ou l’isolation entre les organisations. La combinaison de la gestion des services, de la gestion des données, de l’autonomie et des exigences d’isolation d’une organisation a un impact sur les conteneurs Active Directory utilisés pour déléguer l’administration.  
  
## <a name="isolation-and-autonomy-requirements"></a>Exigences en matière d’isolation et d’autonomie  
Le nombre de forêts que vous devez déployer est basé sur les exigences d’autonomie et d’isolement de chaque groupe au sein de votre organisation. Pour identifier les exigences de conception de votre forêt, vous devez identifier l’autonomie et les exigences d’isolation pour tous les groupes de votre organisation. Plus précisément, vous devez identifier la nécessité de l’isolation des données, de l’autonomie des données, de l’isolation du service et de l’autonomie du service. Vous devez également identifier les zones de connectivité limitée de votre organisation.  
  
### <a name="data-isolation"></a>Isolation des données  
L’isolation des données implique un contrôle exclusif sur les données par le groupe ou l’organisation qui détient les données. Il est important de noter que les administrateurs de service peuvent prendre le contrôle d’une ressource en dehors des administrateurs de données. En plus, les administrateurs de données n’ont pas la possibilité d’empêcher les administrateurs de service d’accéder aux ressources qu’ils contrôlent. Par conséquent, vous ne pouvez pas isoler les données lorsqu’un autre groupe au sein de l’organisation est responsable de l’administration des services. Si un groupe requiert l’isolation des données, ce groupe doit également assumer la responsabilité de l’administration des services.  
  
Étant donné que les données stockées dans AD DS et sur les ordinateurs joints à AD DS ne peuvent pas être isolées des administrateurs de service, le seul moyen pour un groupe au sein d’une organisation d’obtenir une isolation complète des données consiste à créer une forêt distincte pour ces données. Les organisations pour lesquelles les conséquences d’une attaque par un logiciel malveillant ou par un administrateur de services forcé sont importantes peuvent choisir de créer une forêt distincte pour assurer l’isolation des données. Les exigences légales créent généralement un besoin pour ce type d’isolation des données. Exemple :  
  
-   Une institution financière est requise par la loi pour limiter l’accès aux données qui appartiennent à des clients appartenant à une juridiction donnée aux utilisateurs, aux ordinateurs et aux administrateurs situés dans cette juridiction. Bien que l’établissement approuve les administrateurs de service qui travaillent en dehors de la zone protégée, si la limitation d’accès n’est pas respectée, l’établissement n’est plus en mesure de faire de l’entreprise dans cette juridiction. Par conséquent, l’établissement financier doit isoler les données des administrateurs de service en dehors de cette juridiction. Notez que le chiffrement n’est pas toujours une alternative à cette solution. Le chiffrement peut ne pas protéger les données des administrateurs de service.  
  
-   Un entrepreneur de défense est requis par la loi pour limiter l’accès aux données de projet à un ensemble spécifique d’utilisateurs. Bien que le fournisseur approuve les administrateurs de service qui contrôlent les systèmes informatiques associés à d’autres projets, une violation de cette limitation d’accès entraînera la perte de l’activité du fournisseur.  
  
    > [!NOTE]  
    > Si vous avez besoin d’un isolement des données, vous devez décider si vous devez isoler vos données des administrateurs de service ou des administrateurs de données et des utilisateurs ordinaires. Si vos besoins d’isolation sont basés sur l’isolation des administrateurs de données et des utilisateurs ordinaires, vous pouvez utiliser des listes de contrôle d’accès (ACL) pour isoler les données. Dans le cadre de ce processus de conception, l’isolation des administrateurs de données et des utilisateurs ordinaires n’est pas considérée comme une exigence d’isolation des données.  
  
### <a name="data-autonomy"></a>Autonomie des données  
L’autonomie des données implique la capacité d’un groupe ou d’une organisation à gérer ses propres données, y compris à prendre des décisions administratives sur les données et à effectuer toutes les tâches administratives requises sans avoir besoin d’approuver d’une autre autorité.  
  
L’autonomie des données n’empêche pas les administrateurs de service de la forêt d’accéder aux données. Par exemple, un groupe de recherche au sein d’une grande organisation peut être en mesure de gérer ses données propres au projet, mais pas de sécuriser les données d’autres administrateurs de la forêt.  
  
### <a name="service-isolation"></a>Isolation du service  
L’isolation de service implique le contrôle exclusif de l’infrastructure Active Directory. Les groupes qui requièrent l’isolation du service requièrent qu’aucun administrateur en dehors du groupe ne puisse interférer avec le fonctionnement du service d’annuaire.  
  
Les exigences opérationnelles ou légales créent généralement un besoin d’isolation du service. Exemple :  
  
-   Une société de fabrication a une application critique qui contrôle l’équipement en usine. Les interruptions du service sur d’autres parties du réseau de l’organisation ne peuvent pas être autorisées à interférer avec le fonctionnement de l’usine.  
  
-   Une société d’hébergement fournit un service à plusieurs clients. Chaque client requiert l’isolation du service afin que toute interruption de service affectant un client n’affecte pas les autres clients.  
  
### <a name="service-autonomy"></a>Autonomie des services  
L’autonomie de service implique la possibilité de gérer l’infrastructure sans nécessiter de contrôle exclusif ; par exemple, lorsqu’un groupe souhaite apporter des modifications à l’infrastructure (telles que l’ajout ou la suppression de domaines, la modification de l’espace de noms DNS (Domain Name System) ou la modification du schéma) sans l’approbation du propriétaire de la forêt.  
  
L’autonomie du service peut être nécessaire au sein d’une organisation pour un groupe qui souhaite pouvoir contrôler le niveau de service de AD DS (en ajoutant et en supprimant des contrôleurs de domaine, si nécessaire) ou pour un groupe qui doit être en mesure d’installer des applications compatibles avec l’annuaire. exiger des extensions de schéma.  
  
## <a name="limited-connectivity"></a>Connectivité limitée  
Si un groupe au sein de votre organisation possède des réseaux qui sont séparés par des périphériques qui limitent ou limitent la connectivité entre les réseaux (tels que les pare-feu et les périphériques de traduction d’adresses réseau (NAT)), cela peut avoir un impact sur la conception de votre forêt. Lorsque vous identifiez les exigences de conception de votre forêt, veillez à noter les emplacements où vous disposez d’une connectivité réseau limitée. Ces informations sont requises pour vous permettre de prendre des décisions concernant la conception de la forêt.  
  



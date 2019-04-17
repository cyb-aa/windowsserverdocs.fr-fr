---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: "Examen des modèles de domaine"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c57c23c0bd8694b66ab29b45bd9e47826ed1e01d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="reviewing-the-domain-models"></a>Examen des modèles de domaine

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Le modèle de conception de domaine que vous sélectionnez un impact sur les facteurs suivants:  
  
-   Montant de la capacité disponible sur votre réseau que vous êtes disposé à allouer aux Services de domaine ActiveDirectory (ADDS). L’objectif est de sélectionner un modèle qui permet une réplication efficace des informations avec un impact minimal sur la bande passante réseau disponible.  
  
-   Nombre d’utilisateurs dans votre organisation. Si votre organisation comprend un grand nombre d’utilisateurs, déploiement de plusieurs domaines vous permet de partitionner vos données et vous donne davantage de contrôle sur la quantité de trafic de réplication qui transmettra via une connexion réseau donnée. Cela permet de contrôler où les données répliquées et réduire la charge créée par le trafic de réplication sur des liaisons lentes dans votre réseau.  
  
La conception la plus simple de domaine est un domaine unique. Dans une conception de domaine unique, toutes les informations sont répliquées vers tous les contrôleurs de domaine. Toutefois, si nécessaire, vous pouvez déployer des domaines régionaux supplémentaires. Cela peut se produire si des parties de l’infrastructure réseau sont connectés par des liaisons lentes, et que le propriétaire de la forêt souhaite pour vous assurer que le trafic de réplication ne dépasse pas la capacité qui a été allouée aux services ADDS.  
  
Il est préférable de limiter le nombre de domaines que vous déployez dans votre forêt. Cela réduit la complexité du déploiement globale et, par conséquent, coût total de possession. Le tableau suivant répertorie les coûts d’administration liées à l’ajout de domaines régionaux.  
  
|Coût|Implications|  
|--------|----------------|  
|Gestion de plusieurs groupes d’administrateur de service|Chaque domaine possède ses propres groupes d’administrateurs de service qui doivent être gérés indépendamment. L’appartenance à ces groupes Administrateurs de services doit être soigneusement contrôlé.|  
|Maintenir la cohérence entre les paramètres de stratégie de groupe qui sont communs à plusieurs domaines|Les paramètres de stratégie de groupe qui doivent être appliquées à l’échelle de la forêt doivent être appliquées séparément pour chaque domaine individuel dans la forêt.|  
|Maintenir la cohérence entre le contrôle d’accès et d’audit des paramètres qui sont communs à plusieurs domaines|Contrôle d’accès et les paramètres qui doivent être appliquées dans la forêt d’audit doivent être appliquées séparément pour chaque domaine individuel dans la forêt.|  
|Augmente la probabilité d’objets déplacement entre domaines|Plus le nombre de domaines, plus la probabilité que les utilisateurs devront déplacer d’un domaine vers un autre. Ce déplacement peut avoir un impact sur les utilisateurs finaux.|  
  
> [!NOTE]  
>  Mot de passe affinée Windows Server2008 et les stratégies de verrouillage de compte peuvent également avoir un impact sur le modèle de conception de domaine que vous sélectionnez. Avant cette version de Windows Server2008, vous pouvez appliquer qu’un seul compte et mot de passe de stratégie de verrouillage, qui est spécifiée dans le domaine de la stratégie de domaine par défaut, à tous les utilisateurs dans le domaine. Par conséquent, si vous souhaitez que le mot de passe et les paramètres de verrouillage de compte pour des ensembles d’utilisateurs, vous deviez créer un filtre de mot de passe ou déployer plusieurs domaines. Vous pouvez maintenant utiliser des stratégies de mot de passe affinées pour spécifier plusieurs stratégies de mot de passe et d’appliquer des restrictions de mot de passe différent et les stratégies de verrouillage de compte à différents groupes d’utilisateurs dans un seul domaine. Pour plus d’informations sur les stratégies de verrouillage de compte et de mot de passe affinée, voir la Step-by-Step Guide pour la Configuration de stratégie de verrouillage de compte et de mot de passe affinées ([https://go.microsoft.com/fwlink/?LinkID=91477](https://go.microsoft.com/fwlink/?LinkID=91477)).  
  
## <a name="single-domain-model"></a>Modèle de domaine unique  
Un modèle de domaine unique est la plus facile à administrer et le moins coûteux à mettre à jour. Il se compose d’une forêt qui contient un seul domaine. Ce domaine est le domaine racine de forêt, et il contient tous les comptes d’utilisateur et de groupe dans la forêt.  
  
Un modèle de forêt de domaine unique permet de réduire la complexité d’administration en fournissant les avantages suivants:  
  
-   Tout contrôleur de domaine peut authentifier un utilisateur dans la forêt.  
  
-   Tous les contrôleurs de domaine peuvent être des catalogues globaux, vous n’avez pas besoin de planifier la sélection élective du serveur de catalogue global.  
  
Dans une forêt à domaine unique, toutes les données d’annuaire sont répliquées vers tous les emplacements géographiques qui hébergent des contrôleurs de domaine. Ce modèle est le plus simple à gérer, il crée également le trafic de réplication de la plupart des modèles de deux domaine. Partitionnement de l’annuaire dans plusieurs domaines limite la réplication d’objets à des zones géographiques spécifiques, mais les résultats de la charge administrative plus.  
  
## <a name="regional-domain-model"></a>Modèle de domaine régional  
Toutes les données d’objet au sein d’un domaine sont répliquées sur tous les contrôleurs de domaine dans ce domaine. Pour cette raison, si votre forêt comprend un grand nombre d’utilisateurs qui sont distribués sur différents sites géographiques connectés par un réseau étendu (WAN), vous devrez peut-être déployer des domaines régionaux pour réduire le trafic de réplication sur les liaisons WAN. Géographiquement en fonction de domaines régionaux peuvent être organisés en fonction de la connectivité du réseau étendu.  
  
Le modèle de domaine régional vous permet de gérer un environnement stable au fil du temps. Les régions utilisées pour définir des domaines de votre modèle sur des éléments stables, tels que les limites continentales de base. Domaines en fonction des autres facteurs, tels que les groupes de l’organisation, peuvent changer fréquemment et vous devrez peut-être restructurer votre environnement.  
  
Le modèle de domaine régional se compose d’un domaine racine de forêt et un ou plusieurs domaines régionaux. Création d’une conception de modèle de domaine régional implique l’identification quel domaine est le domaine racine de forêt et le nombre de domaines supplémentaires qui sont nécessaires pour répondre à vos besoins de réplication. Si votre organisation inclut les groupes qui nécessitent l’isolation des données ou l’isolation des services à partir d’autres groupes de l’organisation, créez une forêt distincte pour ces groupes. Domaines ne fournissent pas d’isolation des données ou l’isolation du service.  
  



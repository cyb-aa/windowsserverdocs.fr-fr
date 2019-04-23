---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Examen des modèles de domaine
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 23c1eb66eeecab8df63cbd7910a9398bc4e3c705
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870920"
---
# <a name="reviewing-the-domain-models"></a>Examen des modèles de domaine

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Le modèle de conception de domaine que vous sélectionnez un impact sur les facteurs suivants :  
  
- Quantité de capacité disponible sur votre réseau que vous êtes disposé à allouer aux Services de domaine Active Directory (AD DS). L’objectif est de sélectionner un modèle qui fournit une réplication efficace des informations avec un impact minimal sur la bande passante réseau disponible.  

- Nombre d’utilisateurs dans votre organisation. Si votre organisation comprend un grand nombre d’utilisateurs, le déploiement de plusieurs domaines vous permet de partitionner vos données et vous donne davantage de contrôle sur la quantité de trafic de réplication qui est passés via une connexion réseau donnée. Cela rend possible pour contrôler où les données répliquées et de réduire la charge créée par le trafic de réplication sur des liaisons lentes dans votre réseau.  

La conception de domaine la plus simple est un domaine unique. Dans un modèle de domaine unique, toutes les informations sont répliquées vers tous les contrôleurs de domaine. Toutefois, si nécessaire, vous pouvez déployer des domaines régionaux supplémentaires. Cela peut se produire si des parties de l’infrastructure réseau sont connectés par des liaisons lentes, et souhaite que le propriétaire de la forêt pour vous assurer que le trafic de réplication ne dépasse pas la capacité a été allouée dans les services AD DS.  

Il est préférable de réduire le nombre de domaines que vous déployez dans votre forêt. Cela réduit la complexité globale du déploiement et, par conséquent, réduit le coût total de possession. Le tableau suivant répertorie les coûts d’administration liés à l’ajout de domaines régionaux.  

|Coût|Implications|  
|--------|----------------|  
|Gestion de plusieurs groupes d’administrateur de service|Chaque domaine possède ses propres groupes d’administrateur de service qui doivent être gérés indépendamment. L’appartenance de ces groupes d’administrateur de service doit être soigneusement contrôlé.|  
|Maintenir la cohérence parmi les paramètres de stratégie de groupe qui sont communes à plusieurs domaines|Les paramètres de stratégie de groupe qui doivent être appliquées à l’échelle de la forêt doivent être appliquées séparément à chaque domaine individuel dans la forêt.|  
|Maintenir la cohérence entre le contrôle d’accès et les paramètres qui sont communes à plusieurs domaines d’audit|Contrôle d’accès et les paramètres qui doivent être appliquées dans la forêt d’audit doivent être appliquées séparément à chaque domaine individuel dans la forêt.|  
|Probabilité accrue d’objets déplacement entre domaines|Plus le nombre de domaines, plus la probabilité que les utilisateurs doivent déplacer d’un domaine vers un autre. Ce déplacement peut avoir un impact sur les utilisateurs finaux.|  

> [!NOTE]  
> Mot de passe affinée de Windows Server et les stratégies de verrouillage de compte peuvent également avoir un impact sur le modèle de conception de domaine que vous sélectionnez. Avant cette version de Windows Server 2008, vous pouvez appliquer qu’un seul mot de passe et compte de stratégie de verrouillage, qui est spécifiée dans la stratégie de domaine par défaut du domaine, à tous les utilisateurs dans le domaine. Par conséquent, si vous souhaitiez autre mot de passe et les paramètres de verrouillage de compte pour différents groupes d’utilisateurs, vous deviez créer un filtre de mot de passe ou déployer plusieurs domaines. Vous pouvez maintenant utiliser des stratégies de mot de passe affinées pour spécifier plusieurs stratégies de mot de passe et d’appliquer des restrictions de mot de passe différent et les stratégies de verrouillage de compte à différents groupes d’utilisateurs au sein d’un domaine unique. Pour plus d’informations sur les stratégies de verrouillage de compte et de mot de passe affinée, consultez l’article [Guide pas à pas pour la Configuration de stratégie de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modèle de domaine unique

Un modèle de domaine unique est la plus facile à administrer et le moins cher à entretenir. Il se compose d’une forêt qui contient un seul domaine. Ce domaine est le domaine racine de forêt, et il contient tous les comptes d’utilisateur et groupe dans la forêt.  

Un modèle de forêt à domaine unique réduit la complexité administrative en fournissant les avantages suivants :  

- Tout contrôleur de domaine peut authentifier un utilisateur dans la forêt.  

- Tous les contrôleurs de domaine peuvent être des catalogues globaux, vous n’avez pas besoin de planifier le placement des serveurs de catalogue global.  
  
Dans une forêt à domaine unique, toutes les données d’annuaire est répliquée sur tous les emplacements géographiques qui hébergent des contrôleurs de domaine. Bien que ce modèle est la plus facile à gérer, il crée également le trafic de réplication de la plupart des modèles de deux domaines. Le répertoire de partitionnement dans plusieurs domaines limite la réplication d’objets à des régions géographiques spécifiques, mais entraîne une charge administrative plus importante.  
  
## <a name="regional-domain-model"></a>Modèle de domaine régional

Toutes les données d’objet dans un domaine sont répliquées sur tous les contrôleurs de domaine dans ce domaine. Pour cette raison, si votre forêt comprend un grand nombre d’utilisateurs qui sont réparties entre les différents emplacements géographiques connectés par un réseau étendu (WAN), vous devrez peut-être déployer des domaines régionaux pour réduire le trafic de réplication sur les liaisons WAN. Géographiquement en fonction de domaines régionaux peuvent être organisés en fonction de la connectivité du réseau étendu.  
  
Le modèle de domaine régional vous permet de gérer un environnement stable au fil du temps. Les régions utilisées pour définir des domaines dans votre modèle sur des éléments stables, tels que les limites continentales de base. Domaines en fonction des autres facteurs, tels que des groupes au sein de l’organisation, peuvent changer fréquemment et peuvent vous amener à restructurer votre environnement.  
  
Le modèle de domaine régional se compose d’un domaine racine de forêt et un ou plusieurs domaines régionaux. Création d’une conception de modèle de domaine régional s’implique d’identifier quel domaine est le domaine racine de forêt et de déterminer le nombre de domaines supplémentaires qui sont nécessaires pour répondre à vos besoins de réplication. Si votre organisation comprend des groupes qui requièrent l’isolation des données ou l’isolation de service à partir d’autres groupes de l’organisation, créez une forêt distincte pour ces groupes. Domaines ne fournissent pas d’isolation des données ou l’isolation de service.  

---
ms.assetid: e727a33d-133b-43c9-b6a4-7c00f9cb6000
title: Examen des modèles de domaine
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: b77f634d8548994d2f9e130faad9ca34aa226327
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80821952"
---
# <a name="reviewing-the-domain-models"></a>Examen des modèles de domaine

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les facteurs suivants ont un impact sur le modèle de conception de domaine que vous sélectionnez :  
  
- Quantité de capacité disponible sur votre réseau que vous êtes prêt à allouer à Active Directory Domain Services (AD DS). L’objectif est de sélectionner un modèle qui fournit une réplication efficace des informations avec un impact minimal sur la bande passante réseau disponible.  

- Nombre d’utilisateurs de votre organisation. Si votre organisation inclut un grand nombre d’utilisateurs, le déploiement de plusieurs domaines vous permet de partitionner vos données et vous donne davantage de contrôle sur la quantité de trafic de réplication qui passera par une connexion réseau donnée. Vous pouvez ainsi contrôler où les données sont répliquées et réduire la charge créée par le trafic de réplication sur les liaisons lentes de votre réseau.  

La conception de domaine la plus simple est un domaine unique. Dans une conception à domaine unique, toutes les informations sont répliquées sur tous les contrôleurs de domaine. Toutefois, si nécessaire, vous pouvez déployer des domaines régionaux supplémentaires. Cela peut se produire si des parties de l’infrastructure réseau sont connectées par des liaisons lentes, et si le propriétaire de la forêt souhaite s’assurer que le trafic de réplication ne dépasse pas la capacité allouée à AD DS.  

Il est préférable de réduire le nombre de domaines que vous déployez dans votre forêt. Cela réduit la complexité globale du déploiement et, par conséquent, réduit le coût total de possession. Le tableau suivant répertorie les coûts d’administration associés à l’ajout de domaines régionaux.  

|Coût|Différentes|  
|--------|----------------|  
|Gestion de plusieurs groupes d’administrateurs de service|Chaque domaine a ses propres groupes d’administrateurs de service qui doivent être gérés indépendamment. L’appartenance de ces groupes d’administrateurs de service doit être contrôlée avec soin.|  
|Maintien de la cohérence entre les paramètres de stratégie de groupe communs à plusieurs domaines|Stratégie de groupe paramètres qui doivent être appliqués à l’ensemble de la forêt doivent être appliqués séparément à chaque domaine de la forêt.|  
|Maintien de la cohérence entre les paramètres de contrôle d’accès et d’audit communs à plusieurs domaines|Les paramètres de contrôle d’accès et d’audit qui doivent être appliqués dans la forêt doivent être appliqués séparément à chaque domaine de la forêt.|  
|Probabilité accrue d’objets se déplaçant entre des domaines|Plus le nombre de domaines est élevé, plus il est probable que les utilisateurs doivent passer d’un domaine à un autre. Ce déplacement peut potentiellement avoir un impact sur les utilisateurs finaux.|  

> [!NOTE]  
> Les stratégies de verrouillage de compte et de mot de passe affinées Windows Server peuvent également avoir un impact sur le modèle de conception de domaine que vous sélectionnez. Avant cette version de Windows Server 2008, vous pouviez appliquer un seul mot de passe et une seule stratégie de verrouillage de compte, spécifiée dans la stratégie de domaine par défaut du domaine, à tous les utilisateurs du domaine. Par conséquent, si vous souhaitez des paramètres de mot de passe et de verrouillage de compte différents pour différents groupes d’utilisateurs, vous deviez créer un filtre de mot de passe ou déployer plusieurs domaines. Vous pouvez maintenant utiliser des stratégies de mot de passe affinées pour spécifier plusieurs stratégies de mot de passe et appliquer des restrictions de mot de passe et des stratégies de verrouillage de compte différentes à différents groupes d’utilisateurs au sein d’un même domaine. Pour plus d’informations sur le mot de passe affiné et les stratégies de verrouillage de compte, consultez l’article [Guide pas à pas pour la configuration des stratégies de verrouillage de compte et de mot de passe affinées](https://go.microsoft.com/fwlink/?LinkID=91477).  

## <a name="single-domain-model"></a>Modèle à domaine unique

Un modèle de domaine unique est le plus facile à administrer et le moins onéreux à gérer. Il se compose d’une forêt qui contient un seul domaine. Ce domaine est le domaine racine de la forêt et contient tous les comptes d’utilisateurs et de groupes de la forêt.  

Un modèle de forêt à domaine unique réduit la complexité administrative en offrant les avantages suivants :  

- N’importe quel contrôleur de domaine peut authentifier n’importe quel utilisateur de la forêt.  

- Tous les contrôleurs de domaine peuvent être des catalogues globaux. vous n’avez donc pas besoin de planifier le placement du serveur de catalogue global.  
  
Dans une forêt à domaine unique, toutes les données d’annuaire sont répliquées sur tous les emplacements géographiques qui hébergent les contrôleurs de domaine. Bien que ce modèle soit le plus facile à gérer, il crée également le trafic de réplication le plus élevé des deux modèles de domaine. Le partitionnement de l’annuaire en plusieurs domaines limite la réplication des objets à des régions géographiques spécifiques, mais entraîne une charge administrative plus importante.  
  
## <a name="regional-domain-model"></a>Modèle de domaine régional

Toutes les données d’objet d’un domaine sont répliquées sur tous les contrôleurs de domaine de ce domaine. Pour cette raison, si votre forêt comprend un grand nombre d’utilisateurs répartis sur différents emplacements géographiques connectés par un réseau étendu (WAN), vous devrez peut-être déployer des domaines régionaux pour réduire le trafic de réplication sur les liaisons WAN. Les domaines régionaux géographiquement peuvent être organisés en fonction de la connectivité WAN réseau.  
  
Le modèle de domaine régional vous permet de gérer un environnement stable dans le temps. Basez les régions utilisées pour définir des domaines dans votre modèle sur des éléments stables, tels que les limites continentales. Les domaines basés sur d’autres facteurs, tels que les groupes au sein de l’organisation, peuvent changer fréquemment et peuvent vous obliger à restructurer votre environnement.  
  
Le modèle de domaine régional se compose d’un domaine racine de forêt et d’un ou plusieurs domaines régionaux. La création d’une conception de modèle de domaine régional implique l’identification du domaine racine de la forêt et du nombre de domaines supplémentaires requis pour répondre à vos besoins de réplication. Si votre organisation comprend des groupes qui nécessitent l’isolation des données ou l’isolation des services d’autres groupes de l’organisation, créez une forêt distincte pour ces groupes. Les domaines n’assurent pas l’isolation des données ou l’isolation des services.  

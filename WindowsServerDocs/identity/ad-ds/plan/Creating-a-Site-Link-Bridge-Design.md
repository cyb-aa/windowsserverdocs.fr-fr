---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Création d'une conception de pont lien de sites
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 79e91481c357d05617ee0ddc716e2bf6e90b8b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408961"
---
# <a name="creating-a-site-link-bridge-design"></a>Création d'une conception de pont lien de sites

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un pont de liaison de site connecte deux ou plusieurs liens de sites et active la transitivité entre les liens de sites. Chaque lien de site dans un pont doit avoir un site en commun avec un autre lien de sites dans le pont. Le vérificateur de cohérence des données (KCC) utilise les informations de chaque lien de site pour calculer le coût de la réplication entre les sites dans un lien de site et les sites dans les autres liens de site du pont. Sans la présence d’un site commun entre les liaisons de sites, le KCC ne peut pas non plus établir de connexions directes entre les contrôleurs de domaine dans les sites qui sont connectés par le même pont de liaison de site.  
  
Par défaut, tous les liens de sites sont transitifs. Nous vous recommandons de conserver la transitivité activée en ne modifiant pas la valeur par défaut de **relier tous les liens de sites** (activé par défaut). Toutefois, vous devez désactiver **relier tous les liens de sites** et terminer une conception de pont lien de sites si :  

- Votre réseau IP n’est pas entièrement routé. Lorsque vous désactivez **relier tous les liens de sites**, tous les liens de sites sont considérés comme non transitives et vous pouvez créer et configurer des objets de pont de liaison de site pour modéliser le comportement de routage réel de votre réseau.  
- Vous devez contrôler le déroulement de la réplication des modifications apportées dans Active Directory Domain Services (AD DS). Si vous désactivez la liaison de **tous les liens de sites** pour le transport IP de lien de site et la configuration d’un pont entre liens de sites, le pont lien de sites devient l’équivalent d’un réseau disjoint. Tous les liens de sites dans le pont lien de sites peuvent Router de manière transitive, mais ils ne sont pas routés en dehors du pont entre liens de sites.  

Pour plus d’informations sur l’utilisation du composant logiciel enfichable sites et services Active Directory pour désactiver le paramètre **relier tous les liens de sites** , consultez l’article [activer ou désactiver des ponts entre liens de sites](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Contrôle du workflow de réplication AD DS

Deux scénarios dans lesquels vous avez besoin d’une conception de pont lien de sites pour contrôler le workflow de réplication incluent le contrôle du basculement de réplication et le contrôle de la réplication via un pare-feu  
  
### <a name="controlling-replication-failover"></a>Contrôle du basculement de la réplication

Si votre organisation dispose d’une topologie de réseau Hub and spoke, vous ne souhaitez généralement pas que les sites satellites créent des connexions de réplication vers d’autres sites satellites si tous les contrôleurs de domaine du site hub échouent. Dans de tels scénarios, vous devez désactiver **relier tous les liens de site** et créer des ponts entre liens de sites afin que les connexions de réplication soient créées entre le site satellite et un autre site concentrateur, à un seul ou deux tronçons, en dehors du site satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Contrôle de la réplication via un pare-feu

Si deux contrôleurs de domaine représentant le même domaine dans deux sites différents sont autorisés à communiquer entre eux uniquement par le biais d’un pare-feu, vous pouvez désactiver **relier tous les liens de sites** et créer des ponts entre liens de sites pour les sites du même côté du pare. Par conséquent, si votre réseau est séparé par des pare-feu, nous vous recommandons de désactiver la transitivité des liens de sites et de créer des ponts entre liens de sites pour le réseau d’un côté du pare-feu. Pour plus d’informations sur la gestion de la réplication via des pare-feu, consultez l’article [Active Directory dans réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=107074).

---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: Création d'une conception de pont lien de sites
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4a194aa2fe2594c518d310cd86549945487d101e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813990"
---
# <a name="creating-a-site-link-bridge-design"></a>Création d'une conception de pont lien de sites

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Un pont entre liens de site connecte deux ou plus de site établit un lien et favorise la transitivité entre liens de sites. Chaque lien de site dans un pont doit avoir un site en commun avec un autre lien de site dans le pont. Le vérificateur de cohérence des connaissances (KCC) utilise les informations sur chaque lien de sites pour calculer le coût de la réplication entre sites dans un seul lien de site et dans les autres liens de sites du pont. Sans la présence d’un site commun entre les liens de site, le KCC également Impossible d’établir des connexions directes entre les contrôleurs de domaine dans les sites qui sont connectés par le pont lien de sites même.  
  
Par défaut, tous les liens de site sont transitives. Nous vous recommandons de conserver la transitivité activée en ne modifiant ne pas la valeur par défaut **relier tous les liens de site** (activé par défaut). Toutefois, vous devrez désactiver **relier tous les liens du site** et une conception de pont de liaison de site si :  

- Votre réseau IP n’est pas entièrement routé. Lorsque vous désactivez **relier tous les liens du site**, tous les liens de site sont considérés comme non transitives, et vous pouvez créer et configurer des objets de pont lien de sites pour modéliser le comportement de routage réel de votre réseau.  
- Vous devez contrôler le flux de réplication des modifications apportées dans les Services de domaine Active Directory (AD DS). En désactivant **relier tous les liens de site** pour le transport IP de lien de site et configurer un pont lien de sites, le pont devient l’équivalent d’un réseau disjoint. Tous les liens de site dans le pont lien de sites peuvent acheminer de manière transitive, mais ils n’acheminent pas en dehors du pont lien de sites.  

Pour plus d’informations sur l’utilisation du composant logiciel enfichable Services et Sites Active Directory pour désactiver le **relier tous les liens de site** définition, consultez l’article [activer ou désactiver des ponts entre liens de site](https://go.microsoft.com/fwlink/?LinkId=107073).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Contrôle du flux de réplication AD DS

Deux scénarios dans lesquels vous avez besoin d’une conception de pont de liaison de site pour contrôler le flux de réplication incluent le contrôle de basculement de la réplication et le contrôle de la réplication via un pare-feu.  
  
### <a name="controlling-replication-failover"></a>Contrôle de basculement de la réplication

Si votre organisation dispose d’une topologie de réseau hub-and-spoke, vous généralement ne souhaitez pas les sites de satellite pour créer des connexions de réplication vers d’autres sites satellite si tous les contrôleurs de domaine dans le site concentrateur échouent. Dans de tels scénarios, vous devez désactiver **relier tous les liens du site** et créer des ponts entre liens de site afin que les connexions de réplication sont créées entre le site satellite et un autre site concentrateur qui est juste une ou deux sauts en dehors du site satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Contrôle de la réplication via un pare-feu

Si deux contrôleurs de domaine qui représente le même domaine dans deux sites différents sont spécifiquement autorisés à communiquer entre eux uniquement par le biais d’un pare-feu, vous pouvez désactiver **relier tous les liens du site** et créer des ponts entre liens de site pour sites du même côté du pare-feu. Par conséquent, si votre réseau est séparé par des pare-feu, nous vous recommandons de désactiver la transitivité de liens de sites et de créer des ponts entre liens de site pour le réseau d’un côté du pare-feu. Pour plus d’informations sur la gestion de la réplication à travers des pare-feu, consultez l’article [Active Directory sur les réseaux segmentés par des pare-feu](https://go.microsoft.com/fwlink/?LinkId=107074).

---
ms.assetid: 64142026-07b5-4601-840a-c8dcf6ab9814
title: "Création d’une conception de pont lien de sites"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 58dda7c1f56fa3799b902ab5458e71323047ec73
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-link-bridge-design"></a>Création d’une conception de pont lien de sites

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Un pont entre liens de site connecte deux ou les liaisons de sites plus et favorise la transitivité entre liens de sites. Chaque lien de sites d’un pont doit disposer d’un site en commun avec un autre lien de site dans le pont. Le vérificateur de cohérence des connaissances (KCC) utilise les informations sur chaque lien de sites pour calculer le coût de la réplication entre sites dans un lien de sites et des sites dans les liens de sites du pont. Sans la présence d’un site commun entre liens de sites, le KCC également Impossible d’établir des connexions directes entre les contrôleurs de domaine dans les sites qui sont connectés par le pont lien de sites même.  
  
Par défaut, tous les liens de sites sont transitives. Nous vous recommandons de conserver la transitivité activée en modifiant ne pas la valeur par défaut **relier tous les liens de sites** (activé par défaut). Toutefois, vous devez désactiver **relier tous les liens de sites** et effectuer une conception de pont de liaison de site si:  
  
-   Votre réseau IP n’est pas entièrement routé. Lorsque vous désactivez **relier tous les liens de sites**, tous les liens de sites sont considérés comme non transitives, et vous pouvez créer et configurer des objets pont lien de sites pour modéliser le comportement du routage réel de votre réseau.  
  
-   Vous avez besoin de contrôler le flux de la réplication des modifications apportées dans les Services de domaine ActiveDirectory (ADDS). En désactivant **relier tous les liens de sites** pour le transport IP de lien de site et la configuration d’un pont entre liens de site, le pont lien de sites est l’équivalent d’un réseau disjoint. Tous les liens de sites dans le pont lien de site peuvent acheminer de manière transitive, mais ils n’achemine pas en dehors du pont lien de sites.  
  
Pour plus d’informations sur la façon d’utiliser le composant logiciel enfichable Services et Sites ActiveDirectory pour désactiver le **relier tous les liens de sites** définition, voir Activer ou désactiver des ponts entre liens de site ([https://go.microsoft.com/fwlink/?LinkId=107073](https://go.microsoft.com/fwlink/?LinkId=107073)).  
  
## <a name="controlling-ad-ds-replication-flow"></a>Contrôler le flux de réplication ADDS  
Deux scénarios dans lesquels vous avez besoin d’une conception de pont de lien de site pour contrôler le flux de réplication incluent le contrôle de basculement de la réplication et le contrôle de la réplication via un pare-feu.  
  
### <a name="controlling-replication-failover"></a>Contrôle de basculement de la réplication  
Si votre organisation dispose d’une topologie hub and spoke réseau, vous généralement ne souhaitez pas que les sites satellite pour créer des connexions de réplication vers d’autres sites satellite si tous les contrôleurs de domaine dans le site concentrateur échouent. Dans ces scénarios, vous devez désactiver **relier tous les liens de sites** et créer des ponts entre liens de site afin que les connexions de réplication sont créées entre le site satellite et un autre site hub est simplement un ou deux sauts en dehors du site satellite.  
  
### <a name="controlling-replication-through-a-firewall"></a>Contrôle de la réplication via un pare-feu  
Si deux contrôleurs de domaine qui représente le même domaine dans différents sites sont spécifiquement autorisés à communiquer entre eux uniquement via un pare-feu, vous pouvez désactiver **relier tous les liens de sites** et créer des ponts entre liens de sites sur le même côté du pare-feu. Par conséquent, si votre réseau est séparé par des pare-feu, nous vous recommandons de désactiver la transitivité de liens de sites et créer des ponts entre liens de site pour le réseau sur un côté du pare-feu. Pour plus d’informations sur la gestion de la réplication via des pare-feu, voir ActiveDirectory sur les réseaux segmentés par des pare-feu ([https://go.microsoft.com/fwlink/?LinkId=107074](https://go.microsoft.com/fwlink/?LinkId=107074)).  
  



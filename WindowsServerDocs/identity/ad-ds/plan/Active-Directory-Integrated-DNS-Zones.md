---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: "Active les Zones DNS intégrées à ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 11940ddda320ea36bf8439afed91fcf6803f2fee
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-integrated-dns-zones"></a>Active les Zones DNS intégrées à ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Serveurs de noms (DNS) de domaine en cours d’exécution sur les contrôleurs de domaine peuvent stocker leurs zones dans les Services de domaine ActiveDirectory (ADDS). De cette façon, il n’est pas nécessaire de configurer une topologie de réplication DNS distincte qui utilise des transferts de zone DNS ordinaires, car toutes les données de zone sont répliquées automatiquement au moyen de la réplication ActiveDirectory. Cela simplifie le processus de déploiement de DNS et offre les avantages suivants:  
  
-   Plusieurs maîtres sont créés pour la réplication DNS. Par conséquent, n’importe quel contrôleur de domaine dans le domaine exécutant le service serveur DNS peut écrire des mises à jour dans les zones DNS intégrées à ActiveDirectory pour le nom de domaine pour lequel ils font autorités. Une topologie de transfert de zone DNS distincte n’est pas nécessaire.  
  
-   Les mises à jour dynamiques sécurisées sont prises en charge. Les mises à jour dynamiques sécurisées permettent à un administrateur contrôler quels ordinateurs mettre à jour les noms et empêcher les ordinateurs non autorisés de remplacer des noms existants dans DNS.  
  
DNS intégré à ActiveDirectory dans Windows Server2008 stocke les données de zone dans les partitions d’annuaire d’application. (Il n’existe aucune modification comportementale à partir de l’intégration de DNS basé sur Windows Server2003 avec ActiveDirectory.) Les partitions d’annuaire DNS-spécifiques suivantes sont créées lors de l’installation des services ADDS:  
  
-   Une partition d’annuaire de forêt, appelée ForestDnsZones  
  
-   Partitions d’annuaire d’applications à l’échelle du domaine pour chaque domaine dans la forêt, nommé DomainDnsZones  
  
Pour plus d’informations sur comment les services ADDS stocke les informations de DNS dans les partitions d’applications, voir la [référence technique DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Nous vous recommandons d’installer DNS lorsque vous exécutez l’Assistant Installation de Services de domaine ActiveDirectory (Dcpromo.exe). Si vous le faites, l’Assistant crée automatiquement la délégation de zone DNS. Pour plus d’informations, voir [déploiement d’un domaine racine de forêt Windows Server2008](https://technet.microsoft.com/library/cc731174.aspx).  
  



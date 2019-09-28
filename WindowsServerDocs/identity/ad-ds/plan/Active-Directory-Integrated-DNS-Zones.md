---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zones DNS intégrées à Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 858dbc539d48dc3d024ceb871fa9f2d1f80cce0d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402671"
---
# <a name="active-directory-integrated-dns-zones"></a>Zones DNS intégrées à Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Les serveurs DNS (Domain Name System) exécutés sur des contrôleurs de domaine peuvent stocker leurs zones dans Active Directory Domain Services (AD DS). De cette façon, il n’est pas nécessaire de configurer une topologie de réplication DNS distincte qui utilise les transferts de zone DNS ordinaires, car toutes les données de zone sont répliquées automatiquement au moyen d’Active Directory réplication. Cela simplifie le processus de déploiement de DNS et offre les avantages suivants :  
  
-   Plusieurs maîtres sont créés pour la réplication DNS. Par conséquent, tout contrôleur de domaine dans le domaine qui exécute le service serveur DNS peut écrire des mises à jour dans les zones DNS intégrées au Active Directory pour le nom de domaine pour lequel ils font autorité. Une topologie de transfert de zone DNS distincte n’est pas nécessaire.  
  
-   Les mises à jour dynamiques sécurisées sont prises en charge. Les mises à jour dynamiques sécurisées permettent à un administrateur de contrôler les ordinateurs qui mettent à jour les noms et empêchent les ordinateurs non autorisés de remplacer les noms existants dans DNS.  
  
Le DNS intégré à la Active Directory dans Windows Server 2008 stocke les données de zone dans les partitions de l’annuaire d’applications. (Il n’existe aucune modification comportementale de l’intégration DNS basée sur Windows Server 2003 avec Active Directory.) Les partitions d’annuaire d’applications DNS suivantes sont créées lors de l’installation de AD DS :  
  
-   Une partition de l’annuaire d’applications à l’ensemble de la forêt, appelée ForestDnsZones  
  
-   Partitions de l’annuaire d’applications à l’ensemble du domaine pour chaque domaine de la forêt, nommé DomainDnsZones  
  
Pour plus d’informations sur la façon dont AD DS stocke les informations DNS dans des partitions d’application, consultez la [référence technique DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Nous vous recommandons d’installer le DNS quand vous exécutez le Assistant Installation Active Directory Domain Services (Dcpromo. exe). Dans ce cas, l’Assistant crée automatiquement la délégation de zone DNS. Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  



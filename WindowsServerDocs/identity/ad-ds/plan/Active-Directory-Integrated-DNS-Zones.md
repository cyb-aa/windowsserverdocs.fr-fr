---
ms.assetid: 39c0126d-af5e-4dcb-88c1-aa38f888e973
title: Zones DNS intégrées à Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 5e0830944ec5d91dace52db337e89211ee362b98
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873250"
---
# <a name="active-directory-integrated-dns-zones"></a>Zones DNS intégrées à Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Serveurs de noms (DNS) de domaine en cours d’exécution sur les contrôleurs de domaine peuvent stocker leurs zones dans les Services de domaine Active Directory (AD DS). De cette façon, il n’est pas nécessaire de configurer une topologie de réplication DNS distincte qui utilise des transferts de zone DNS ordinaires, car toutes les données de zone sont répliquées automatiquement au moyen de la réplication Active Directory. Cela simplifie le processus de déploiement DNS et que vous offre les avantages suivants :  
  
-   Plusieurs maîtres sont créés pour la réplication DNS. Par conséquent, n’importe quel contrôleur de domaine du domaine exécutant le service serveur DNS peut écrire des mises à jour dans les zones DNS intégrées à Active Directory pour le nom de domaine pour lequel ils font autorités. Une topologie de transfert de zone DNS distincte n’est pas nécessaire.  
  
-   Les mises à jour dynamiques sécurisées sont prises en charge. Les mises à jour dynamiques sécurisées permettent à un administrateur contrôler quels ordinateurs mettre à jour les noms et d’empêcher des ordinateurs non autorisés de remplacer des noms existants dans DNS.  
  
DNS Active Directory-intégré dans Windows Server 2008 stocke les données de zone dans les partitions d’annuaire d’application. (Il n’existe aucun des changements de comportement de l’intégration de DNS basé sur Windows Server 2003 avec Active Directory.) Les partitions d’annuaire application DNS spécifiques suivantes sont créées lors de l’installation des services AD DS :  
  
-   Une partition d’annuaire de forêt, appelée ForestDnsZones  
  
-   Partitions d’annuaire du domaine pour chaque domaine dans la forêt, nommé DomainDnsZones  
  
Pour plus d’informations sur comment les services AD DS stocke les informations DNS dans les partitions d’application, consultez le [référence technique DNS](https://go.microsoft.com/fwlink/?LinkId=106636).  
  
> [!NOTE]  
> Nous vous recommandons d’installer DNS lorsque vous exécutez l’Active Directory domaine Assistant Installation des Services (Dcpromo.exe). Si vous procédez ainsi, l’Assistant crée automatiquement la délégation de zone DNS. Pour plus d’informations, consultez [déploiement d’un domaine racine de forêt Windows Server 2008](https://technet.microsoft.com/library/cc731174.aspx).  
  



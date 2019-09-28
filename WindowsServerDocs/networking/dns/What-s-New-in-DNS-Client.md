---
title: Nouveautés du client DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités du client DNS dans Windows Server et Windows 10
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 696ea0b499a4132d630cc0cda15a1d7efdac37a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356059"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Nouveautés du client DNS dans Windows Server 2016

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit les nouveautés et les modifications apportées à la fonctionnalité du client DNS (Domain Name System) dans Windows 10 et Windows Server 2016 et les versions ultérieures de ces systèmes d’exploitation.
  
## <a name="updates-to-dns-client"></a>Mises à jour du client DNS

**Liaison de service du client DNS**: Dans Windows 10, le service client DNS offre une prise en charge améliorée pour les ordinateurs dotés de plusieurs interfaces réseau. Pour les ordinateurs multirésidents, la résolution DNS est optimisée des manières suivantes :  
  
-   Lorsqu’un serveur DNS configuré sur une interface spécifique est utilisé pour résoudre une requête DNS, le service client DNS crée une liaison avec cette interface avant d’envoyer la requête DNS.  
  
    En liant à une interface spécifique, le client DNS peut clairement spécifier l’interface dans laquelle la résolution de noms se produit, ce qui permet aux applications d’optimiser les communications avec le client DNS sur cette interface réseau.  
  
-   Si le serveur DNS utilisé est désigné par un paramètre de stratégie de groupe à partir de la table de stratégie de résolution de noms (NRPT), le service client DNS n’est pas lié à une interface spécifique.  
  
> [!NOTE]  
> Les modifications apportées au service client DNS dans Windows 10 sont également présentes sur les ordinateurs exécutant Windows Server 2016 et versions ultérieures.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés du serveur DNS dans Windows Server 2016](What-s-New-in-DNS-Server.md)  
  


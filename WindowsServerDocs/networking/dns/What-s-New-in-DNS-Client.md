---
title: Quelles sont les nouveautés dans le Client DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités de Client DNS dans Windows Server et Windows10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: defe88fe2a1e4f5393be4d5d5938d9f5bfbfb5d6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Quelles sont les nouveautés dans le Client DNS dans Windows Server 2016

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique décrit les fonctionnalités de client de système DNS (Domain Name) qui sont nouvelles ou modifiées dans Windows10 et Windows Server2016 et versions ultérieures de ces systèmes d’exploitation.
  
## <a name="updates-to-dns-client"></a>Mises à jour de Client DNS

**Liaison du service Client DNS**: dans Windows 10, le service Client DNS offre une prise en charge améliorée pour les ordinateurs avec plusieurs interfaces réseau. Pour les ordinateurs multirésident, la résolution DNS est optimisée manières suivantes:  
  
-   Lorsqu’un serveur DNS configuré sur une interface spécifique est utilisé pour résoudre une requête DNS, le service Client DNS lie à cette interface avant d’envoyer la requête DNS.  
  
    En le liant à une interface spécifique, le client DNS pouvant clairement spécifier l’interface où la résolution de noms se produit, ce qui permet aux applications pour optimiser les communications avec le client DNS sur cette interface de réseau.  
  
-   Si le serveur DNS qui est utilisé est désigné par un paramètre de stratégie de groupe à partir de Table de stratégie de résolution de nom (NRPT), le service de Client DNS n’est pas lié à une interface spécifique.  
  
> [!NOTE]  
> Modifications apportées au service Client DNS dans Windows10 sont également présentes sur les ordinateurs exécutant Windows Server2016 et versions ultérieures.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés du serveur DNS dans Windows Server 2016](What-s-New-in-DNS-Server.md)  
  


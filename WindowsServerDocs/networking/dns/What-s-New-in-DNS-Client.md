---
title: Quelles sont les nouveautés dans le Client DNS dans Windows Server
description: Cette rubrique fournit une vue d’ensemble des nouvelles fonctionnalités de Client DNS dans Windows Server et Windows 10
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 6edaba84-4595-4fd8-95d7-64d4d975a38a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34b1a64465e217fbd7e6b3ae55e89832a7a4e48c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860560"
---
# <a name="whats-new-in-dns-client-in-windows-server-2016"></a>Nouveautés du client DNS dans Windows Server 2016

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit les fonctionnalités de client de système DNS (Domain Name) qui sont nouvelles ou modifiées dans Windows 10 et Windows Server 2016 et versions ultérieures de ces systèmes d’exploitation.
  
## <a name="updates-to-dns-client"></a>Mises à jour DNS Client

**Liaison de service Client DNS**: Dans Windows 10, le service Client DNS offre une prise en charge améliorée pour les ordinateurs avec plusieurs interfaces réseau. Pour les ordinateurs à hébergement multiple, la résolution DNS est optimisée comme suit :  
  
-   Lorsqu’un serveur DNS configuré sur une interface spécifique est utilisé pour résoudre une requête DNS, le service Client DNS est lié à cette interface avant d’envoyer la requête DNS.  
  
    En liant clairement vers une interface spécifique, le client DNS peut spécifier l’interface où la résolution de noms se produit, l’activation d’applications pour optimiser les communications avec le client DNS sur cette interface de réseau.  
  
-   Si le serveur DNS qui est utilisé est désigné par un paramètre de stratégie de groupe à partir de Table de stratégie de résolution de noms (NRPT), le service Client DNS ne lie pas vers une interface spécifique.  
  
> [!NOTE]  
> Les modifications du service Client DNS dans Windows 10 sont également présentes sur les ordinateurs exécutant Windows Server 2016 et versions ultérieures.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Nouveautés du serveur DNS dans Windows Server 2016](What-s-New-in-DNS-Server.md)  
  


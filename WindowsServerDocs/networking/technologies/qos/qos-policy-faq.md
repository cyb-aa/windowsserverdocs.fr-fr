---
title: Forum aux questions QoS
description: Cette rubrique fournit des réponses aux questions sur la stratégie de qualité de service (QoS) dans Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bde6c859284fbd1756517d7b08ec96816bb2bb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395958"
---
# <a name="qos-policy-frequently-asked-questions"></a>Forum aux questions sur la stratégie QoS

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Voici les questions fréquemment posées et les réponses à ces questions – pour la stratégie QoS.
  
1.  **Quel système d’exploitation mon contrôleur de domaine doit-il exécuter pour utiliser la stratégie QoS ?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008

2.  **Quels systèmes d’exploitation prennent en charge l’application de la stratégie QoS à l’utilisateur ou à l’ordinateur ?**

     Vous pouvez appliquer des stratégies de QoS aux utilisateurs ou ordinateurs exécutant Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista.

3.  **Les stratégies de qualité de service s’appliquent-elles à l’expéditeur ou au récepteur du trafic ?**

     Les stratégies de qualité de service doivent être appliquées sur l’ordinateur expéditeur pour affecter le trafic sortant. Pour affecter le trafic bidirectionnel de deux ordinateurs, les stratégies de qualité de service doivent être appliquées aux deux ordinateurs.

4.  **Que se passe-t-il si des stratégies de QoS conflictuelles sont déployées sur le même ordinateur ?**  
  
     Si plusieurs stratégies s’appliquent, la stratégie QoS la plus spécifique est prioritaire. Par exemple, une stratégie qui indique qu’une adresse d’hôte (192.168.4.12) est appliquée à la place d’une adresse réseau moins spécifique (192.168.0.0/16). Si une stratégie au niveau de l’ordinateur et de l’utilisateur a la même spécificité, la stratégie QoS au niveau de l’utilisateur est appliquée à la place de la stratégie QoS au niveau de l’ordinateur. 

5.  **La stratégie QoS est-elle activée par défaut ?**

     Non, la stratégie QoS n’est pas activée par défaut. Vous devez créer des stratégies de QoS manuellement pour activer la qualité de service.  Pour plus d’informations, consultez [gérer la stratégie QoS](qos-policy-manage.md).

Pour obtenir la première rubrique de ce guide, consultez [stratégie de qualité de service (QoS)](qos-policy-top.md).

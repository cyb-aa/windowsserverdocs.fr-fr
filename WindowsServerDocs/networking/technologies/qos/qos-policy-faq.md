---
title: Forum aux Questions de qualité de service
description: Cette rubrique fournit des réponses aux questions sur les stratégies de qualité de Service (QoS) dans Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833750"
---
# <a name="qos-policy-frequently-asked-questions"></a>Questions fréquentes de stratégie de QoS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Éléments suivants sont Forum aux questions – et des réponses à ces questions : pour la stratégie QoS.
  
1.  **Quel système d’exploitation mon contrôleur de domaine doit-elle exécuter pour utiliser la stratégie de qualité de service ?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008

2.  **Quels systèmes d’exploitation prennent en charge l’application de la stratégie de QoS à l’utilisateur ou d’un ordinateur ?**

     Vous pouvez appliquer des stratégies de QoS aux utilisateurs ou ordinateurs qui exécutent Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 et Windows Vista.

3.  **Stratégies de QoS s’appliquent à l’expéditeur ou récepteur de trafic ?**

     Stratégies de QoS doivent être appliquées sur l’ordinateur émetteur à affecter son trafic sortant. Pour affecter le trafic bidirectionnel de deux ordinateurs, les stratégies de QoS doivent être appliquées aux deux ordinateurs.

4.  **Que se passe-t-il si les stratégies de qualité de service en conflit sont déployés sur le même ordinateur ?**  
  
     Si plusieurs stratégies s’appliquent, la stratégie de QoS plus spécifique est prioritaire. Par exemple, une stratégie indiquant une adresse d’hôte (192.168.4.12) est appliquée au lieu d’une adresse réseau moins spécifique (192.168.0.0/16). Si une stratégie au niveau de l’ordinateur et au niveau de l’utilisateur ont la spécificité de même, la stratégie de QoS au niveau de l’utilisateur est appliquée au lieu de la stratégie de QoS au niveau de l’ordinateur. 

5.  **Stratégie de QoS est activée par défaut ?**

     Non, stratégie de QoS n’est pas activée par défaut. Vous devez créer des stratégies de QoS manuellement pour activer la qualité de service.  Pour plus d’informations, consultez [gérer la stratégie de QoS](qos-policy-manage.md).

Pour la première rubrique de ce guide, consultez [stratégie de qualité de Service (QoS)](qos-policy-top.md).

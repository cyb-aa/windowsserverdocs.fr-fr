---
title: Forum aux Questions de QoS
description: Cette rubrique fournit des réponses aux questions sur la stratégie de qualité de Service (QoS) dans Windows Server2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>Stratégie de QoS Forum aux Questions

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Suivantes sont Forum aux questions – et les réponses à ces questions: pour la stratégie de QoS.
  
1.  **Quel système d’exploitation le contrôleur de domaine a-t-elle besoin d’être en cours d’exécution pour utiliser une stratégie de qualité de service?**
  
     Windows Server2016, Windows Server2012R2, Windows Server2012, Windows Server2008R2 ou Windows Server2008

2.  **Quels systèmes d’exploitation prennent en charge l’application de stratégie de QoS à l’utilisateur ou l’ordinateur?**

     Vous pouvez appliquer des stratégies de QoS à des utilisateurs ou des ordinateurs exécutant Windows Server2016, Windows10, Windows Server2012R2, Windows8.1, Windows Server2012, Windows8, Windows Server2008R2, Windows Server2008 et WindowsVista.

3.  **Stratégies de QoS s’appliquent à l’expéditeur ou du récepteur de trafic?**

     Stratégies de QoS doivent être appliquées sur l’ordinateur émetteur d’affecter le trafic sortant. Pour affecter le trafic bidirectionnel de deux ordinateurs, les stratégies de QoS doivent être appliquées aux deux ordinateurs.

4.  **Que se passe-t-il si les stratégies de qualité de service en conflit sont déployées sur le même ordinateur?**  
  
     Si plusieurs stratégies s’appliquent, la stratégie de QoS plus spécifique est prioritaire. Par exemple, une stratégie de l’ordinateur hôte, les États d’adresses (192.168.4.12) est appliqué au lieu d’un moins spécifique réseau adresse (192.168.0.0/16). Si une stratégie au niveau de l’ordinateur et au niveau de l’utilisateur ont la même spécificité, la stratégie de QoS au niveau de l’utilisateur est appliquée au lieu de la stratégie de QoS au niveau de l’ordinateur. 

5.  **Stratégie de QoS est activée par défaut?**

     Non, stratégie de QoS n’est pas activée par défaut. Vous devez créer des stratégies de QoS manuellement pour activer la qualité de service.  Pour plus d’informations, voir [gérer la stratégie de QoS](qos-policy-manage.md).

Pour la première rubrique de ce guide, voir [stratégie de qualité de Service (QoS)](qos-policy-top.md).

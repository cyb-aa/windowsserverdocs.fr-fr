---
title: Stratégie de groupe d’actualisation
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4d9f5d38199f8cf3c0ffe46df4cd975cd9c56ff6
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="refresh-group-policy"></a>Stratégie de groupe d’actualisation

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour actualiser manuellement la stratégie de groupe sur l’ordinateur local. Lorsque la stratégie de groupe est actualisée, si l’inscription automatique de certificat est configuré et fonctionne correctement, l’ordinateur local est inscrit automatiquement un certificat par l’autorité de certification (CA).  
  
> [!NOTE]  
> Stratégie de groupe est actualisée automatiquement lorsque vous redémarrez l’ordinateur membre du domaine ou lorsqu’un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, les stratégie de groupe est actualisée régulièrement. Par défaut, cette actualisation périodique est effectuée toutes les 90minutes avec un décalage aléatoire de 30minutes.  
  
L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Pour actualiser la stratégie de groupe sur l’ordinateur local  
  
1.  Sur l’ordinateur sur lequel le serveur NPS est installé, ouvrez Windows PowerShell&reg; à l’aide de l’icône de la barre des tâches.  
  
2.  À l’invite Windows PowerShell, tapez **gpupdate**, puis appuyez sur ENTRÉE.  
  



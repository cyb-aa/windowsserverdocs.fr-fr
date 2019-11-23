---
title: Actualiser la stratégie de groupe
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 88d39f41f61ae7c7f6a1fb84aa99806c4796c8cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356186"
---
# <a name="refresh-group-policy"></a>Actualiser la stratégie de groupe

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez appliquer cette procédure pour actualiser manuellement la stratégie de groupe sur l'ordinateur local. Lorsque stratégie de groupe est actualisé, si l’inscription automatique des certificats est configurée et fonctionne correctement, l’ordinateur local est inscrit automatiquement auprès de l’autorité de certification (CA).  
  
> [!NOTE]  
> La stratégie de groupe est actualisée automatiquement lorsque vous redémarrez l'ordinateur membre du domaine ou lorsqu'un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, stratégie de groupe est régulièrement actualisé. Par défaut, cette actualisation périodique est effectuée toutes les 90 minutes avec un décalage aléatoire pouvant atteindre 30 minutes.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Pour actualiser la stratégie de groupe sur l'ordinateur local  
  
1.  Sur l’ordinateur sur lequel NPS est installé, ouvrez Windows PowerShell&reg; à l’aide de l’icône dans la barre des tâches.  
  
2.  À l’invite de commandes Windows PowerShell, tapez **gpupdate**, puis appuyez sur entrée.  
  



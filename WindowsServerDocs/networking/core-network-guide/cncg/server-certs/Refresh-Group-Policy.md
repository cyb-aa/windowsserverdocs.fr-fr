---
title: Actualiser la stratégie de groupe
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: 65b36794-bb09-4c1b-a2e7-8fc780893d97
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 83dd48297535aafe30e48fe37010d81b279f4c91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863450"
---
# <a name="refresh-group-policy"></a>Actualiser la stratégie de groupe

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez appliquer cette procédure pour actualiser manuellement la stratégie de groupe sur l'ordinateur local. Lorsque la stratégie de groupe est actualisée, si l’inscription automatique est configurée et fonctionne correctement, l’ordinateur local est inscrit automatiquement un certificat par l’autorité de certification (CA).  
  
> [!NOTE]  
> La stratégie de groupe est actualisée automatiquement lorsque vous redémarrez l'ordinateur membre du domaine ou lorsqu'un utilisateur ouvre une session sur un ordinateur membre du domaine. En outre, les stratégie de groupe est régulièrement actualisée. Par défaut, cette actualisation périodique est effectuée toutes les 90 minutes avec un décalage aléatoire de 30 minutes.  
  
Pour mener à bien cette procédure, il faut appartenir au groupe **Administrateurs** ou à un groupe équivalent.  
  
### <a name="to-refresh-group-policy-on-the-local-computer"></a>Pour actualiser la stratégie de groupe sur l'ordinateur local  
  
1.  Sur l’ordinateur où le serveur NPS est installé, ouvrez Windows PowerShell&reg; à l’aide de l’icône dans la barre des tâches.  
  
2.  À l’invite Windows PowerShell, tapez **gpupdate**, puis appuyez sur ENTRÉE.  
  



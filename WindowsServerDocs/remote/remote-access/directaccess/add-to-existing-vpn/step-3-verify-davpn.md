---
title: Étape 3 vérifier le déploiement
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement d’accès à distance (VPN) existant pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 82acca0309835149ca35676784120b3130e72a54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388696"
---
# <a name="step-3-verify-the-deployment"></a>Étape 3 vérifier le déploiement

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Pour vérifier l’accès aux ressources internes via DirectAccess  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
3.  Cliquez sur le**connexion DirectAccess**et vous verrez que l'état est**connecté localement**.  
  
4.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  



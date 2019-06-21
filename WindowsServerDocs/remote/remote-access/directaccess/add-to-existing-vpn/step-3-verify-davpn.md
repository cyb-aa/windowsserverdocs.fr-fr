---
title: Étape 3 vérifier le déploiement
description: Cette rubrique fait partie du guide ajouter DirectAccess à un déploiement de l’accès à distance existants (VPN, Virtual Private Network) pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 43ac612e-2e77-418c-8171-ebb2086b7cb6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 639431405635b7b0792b7a73bd9bb524e0827832
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281818"
---
# <a name="step-3-verify-the-deployment"></a>Étape 3 vérifier le déploiement

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Pour vérifier l’accès aux ressources internes via DirectAccess  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez la stratégie de groupe.  
  
2.  Cliquez sur le**connexions réseau**icône dans la zone de notification pour accéder au Gestionnaire de médias DA.  
  
3.  Cliquez sur le**connexion DirectAccess**et vous verrez que l'état est**connecté localement**.  
  
4.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  



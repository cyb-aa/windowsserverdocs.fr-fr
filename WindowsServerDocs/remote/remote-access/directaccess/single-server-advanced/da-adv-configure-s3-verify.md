---
title: Étape 3 vérifier le déploiement avancé de DirectAccess
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique avec des paramètres avancés pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 51ce3fa1a72420f7272141bb5361b20360b7000c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404903"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Étape 3 vérifier le déploiement avancé de DirectAccess

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Pour vérifier l’accès aux ressources internes via DirectAccess  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez l’objet stratégie de groupe.  
  
2.  Cliquez sur l’icône **connexions réseau** dans la zone de notification pour accéder au gestionnaire multimédia DirectAccess.  
  
3.  Cliquez sur **connexion DirectAccess**. vous verrez que l’État est **connecté localement**.  
  
4.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
## <a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 2 : Configuration des serveurs DirectAccess @ no__t-0  
  



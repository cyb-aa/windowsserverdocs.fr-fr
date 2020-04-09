---
title: Étape 3 vérifier le déploiement avancé de DirectAccess
description: Cette rubrique fait partie du guide déployer un serveur DirectAccess unique avec des paramètres avancés pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: ae8bbff0-c981-4bc6-8df1-861621d0627f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: aca9cc008a096b16e01e5adb2608279ac7ded58b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861472"
---
# <a name="step-3-verify-the-advanced-directaccess-deployment"></a>Étape 3 vérifier le déploiement avancé de DirectAccess

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique décrit comment vérifier que vous avez correctement configuré votre déploiement de DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-directaccess"></a>Pour vérifier l’accès aux ressources internes via DirectAccess  
  
1.  Connectez un ordinateur client DirectAccess au réseau d’entreprise et obtenez l’objet stratégie de groupe.  
  
2.  Cliquez sur l’icône **connexions réseau** dans la zone de notification pour accéder au gestionnaire multimédia DirectAccess.  
  
3.  Cliquez sur **connexion DirectAccess**. vous verrez que l’État est **connecté localement**.  
  
4.  Connectez l’ordinateur client au réseau externe et tentez d’accéder aux ressources internes.  
  
    Vous devriez pouvoir accéder à toutes les ressources d’entreprise.  
  
## <a name="previous-step"></a><a name="BKMK_Links"></a>Étape précédente  
  
-   [Étape 2 : configuration des serveurs DirectAccess](Step-2-Configuring-DirectAccess-Servers.md)  
  



---
title: ÉTAPE 2 configurer EDGE1
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b9eb37433e26c174ccae85482163c976577ff479
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388513"
---
# <a name="step-2-configure-edge1"></a>ÉTAPE 2 configurer EDGE1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

La procédure suivante est exécutée sur le serveur DirectAccess :

## <a name="to-configure-directaccess-on-edge1"></a>Pour configurer DirectAccess sur EDGE1
  
1.  Dans l’écran d' **Accueil** , tapez**RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, dans le volet gauche, cliquez sur **configuration**.  
  
3.  Dans le volet central de la console, dans la zone **étape 2 : serveur d’accès à distance** , cliquez sur **modifier**.  
  
4.  Dans l’Assistant **installation du serveur d’accès à distance** , cliquez sur **configuration du préfixe**. Dans la **page Configuration du préfixe** , dans **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez **2001 : DB8:1 : 1000 ::/59**, puis cliquez sur **suivant**.  
  
5.  Cliquez sur **Terminer**.  
  
6.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
7.  Dans la boîte de dialogue vérification de l' **accès à distance** , vérifiez les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.

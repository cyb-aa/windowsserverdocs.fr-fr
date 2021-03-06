---
title: ÉTAPE 2 configurer EDGE1
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8705e69debec2f0a19f5cc010ed5ca254cc56741
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819312"
---
# <a name="step-2-configure-edge1"></a>ÉTAPE 2 configurer EDGE1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

La procédure suivante est exécutée sur le serveur DirectAccess :

## <a name="to-configure-directaccess-on-edge1"></a>Pour configurer DirectAccess sur EDGE1
  
1.  Dans l’écran d' **Accueil** , tapez**RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** apparaît, confirmez que l'action affichée est bien celle que vous souhaitez effectuer, puis cliquez sur **Oui**.  
  
2.  Dans la console Gestion de l’accès à distance, dans le volet gauche, cliquez sur **configuration**.  
  
3.  Dans le volet central de la console, dans la zone **étape 2 : serveur d’accès à distance** , cliquez sur **modifier**.  
  
4.  Dans l’Assistant **installation du serveur d’accès à distance** , cliquez sur **configuration du préfixe**. Dans la **page Configuration du préfixe** , dans **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez **2001 : DB8:1 : 1000 ::/59**, puis cliquez sur **suivant**.  
  
5.  Cliquez sur **Terminer**.  
  
6.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
7.  Dans la boîte de dialogue vérification de l' **accès à distance** , vérifiez les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.

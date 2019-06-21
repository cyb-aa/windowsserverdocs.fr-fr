---
title: ÉTAPE 2 configurer EDGE1
description: Cette rubrique fait partie du Guide de laboratoire de Test - décrire de DirectAccess dans un Cluster avec équilibrage de charge réseau Windows pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 84457351-1ca7-4e7c-8e2c-53d55b1fcdc0
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 03d7d85f730cf792238aef372337030861cd6a9d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281678"
---
# <a name="step-2-configure-edge1"></a>ÉTAPE 2 configurer EDGE1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

La procédure suivante est effectuée sur le serveur DirectAccess :

## <a name="to-configure-directaccess-on-edge1"></a>Pour configurer DirectAccess sur EDGE1
  
1.  Sur le **Démarrer** , tapez**RAMgmtUI.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la console de gestion de l’accès à distance, dans le volet gauche, cliquez sur **Configuration**.  
  
3.  Dans le volet central de la console, dans le **étape 2 : serveur accès à distance** zone, cliquez sur **modifier**.  
  
4.  Dans le **le programme d’installation du serveur d’accès à distance** Assistant, cliquez sur **Configuration du préfixe**. Sur le **Configuration du préfixe** page **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez **2001:db8:1:1000 :: / 59**, puis cliquez sur **suivant** .  
  
5.  Cliquez sur **Terminer**.  
  
6.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
7.  Sur le **révision d’accès à distance** boîte de dialogue, passez en revue les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.

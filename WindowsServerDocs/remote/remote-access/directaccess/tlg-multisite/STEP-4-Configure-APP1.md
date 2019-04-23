---
title: ÉTAPE 4 configurer APP1
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ccfac583f64d40012881a2d3f6a0897beb16c02a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867220"
---
# <a name="step-4-configure-app1"></a>ÉTAPE 4 configurer APP1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Configurer les paramètres de l’adressage et de passerelle IPv6 statiques pour activer l’accès APP1 au sous-réseau Corpnet de 2.  
  
- Pour configurer la passerelle par défaut et le serveur DNS. La configuration multisite utilise l’ordinateur ROUTEUR1 comme une passerelle par défaut. Configurer la passerelle par défaut sur APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Pour configurer la passerelle par défaut et le serveur DNS  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans le **connexions réseau** fenêtre, avec le bouton droit **connexion Ethernet câblée**, puis cliquez sur **propriétés**.  
  
3.  Sur le **propriétés de connexion Ethernet câblée** boîte de dialogue, cliquez sur **Internet Protocol Version 4 (TCP/IPv4)**, puis cliquez sur **propriétés**.  
  
4.  Dans **passerelle par défaut**, type **10.0.0.254**et dans **serveur DNS auxiliaire**, type **10.2.0.1**, puis cliquez sur **OK** .  
  
5.  Sur le **propriétés de connexion Ethernet câblée** boîte de dialogue, cliquez sur **Internet Protocol Version 6 (TCP/IPv6)**, puis cliquez sur **propriétés**.  
  
6.  Dans **passerelle par défaut**, type **2001:db8:1::fe**. Dans **serveur DNS auxiliaire**, type **2001:db8:2::1**, puis cliquez sur **OK**.  
  
7.  Sur le **propriétés de connexion Ethernet câblée** boîte de dialogue, cliquez sur **fermer**, puis fermez le **connexions réseau** fenêtre.  
  



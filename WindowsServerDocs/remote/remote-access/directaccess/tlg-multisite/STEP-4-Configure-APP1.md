---
title: ÉTAPE 4 configurer APP1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc4715fcec778d1fa63ff84e801961572b9cd589
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404783"
---
# <a name="step-4-configure-app1"></a>ÉTAPE 4 configurer APP1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Configurez les paramètres d’adressage IPv6 statique et de passerelle pour activer l’accès APP1 au sous-réseau 2-Corpnet.  
  
- Pour configurer la passerelle par défaut et le serveur DNS. La configuration multisite utilise l’ordinateur ROUTEUR1 comme passerelle par défaut. Configurez la passerelle par défaut sur APP1.  
  
## <a name="to-configure-the-default-gateway-and-dns-server"></a>Pour configurer la passerelle par défaut et le serveur DNS  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur **connexion Ethernet câblée**, puis cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Dans **passerelle par défaut**, tapez **10.0.0.254**, et dans **serveur DNS auxiliaire**, tapez **10.2.0.1**, puis cliquez sur **OK**.  
  
5.  Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **protocole Internet version 6 (TCP/IPv6)** , puis sur **Propriétés**.  
  
6.  Dans **passerelle par défaut**, tapez **2001 : DB8:1 :: Fe**. Dans **serveur DNS auxiliaire**, tapez **2001 : DB8:2 :: 1**, puis cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **Fermer**, puis fermez la fenêtre **connexions réseau** .  
  



---
title: ÉTAPE 4 configurer APP1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 7000e80f-31b1-43c5-b51e-1469d26909e5
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8011d85f742ebde5fc2013bb483546133e1618ec
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861582"
---
# <a name="step-4-configure-app1"></a>ÉTAPE 4 configurer APP1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

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
  



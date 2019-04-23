---
title: ÉTAPE 9 configurer EDGE1
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
ms.assetid: f6e8d85b-de65-43b3-bf3e-ec84471a1fcc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f5d216934f0d09cdef97ce4405161862b112d632
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864870"
---
# <a name="step-9-configure-edge1"></a>ÉTAPE 9 configurer EDGE1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les procédures suivantes sont effectuées sur le serveur EDGE1 :  
  
1. Configurer les serveurs DNS sur EDGE1. Il est nécessaire de configurer le serveur DNS du domaine corp2.corp.contoso.com sur EDGE1.  
  
2. Configurer le routage entre sous-réseaux. Configurer le routage sur EDGE1 pour permettre la communication entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise.  
  
## <a name="IPv6"></a>Configurer les serveurs DNS sur EDGE1  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **Corpnet**, cliquez sur le lien.  
  
2.  Dans la fenêtre Connexions réseau, cliquez sur **Corpnet**, puis cliquez sur **propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
4.  Dans **serveur DNS auxiliaire**, type **10.2.0.1**. puis cliquez sur **OK**.  
  
5.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)**, puis cliquez sur **Propriétés**.  
  
6.  Dans **serveur DNS auxiliaire**, type **2001:db8:2::1** puis cliquez sur **OK**.  
  
7.  Sur le **Corpnet propriétés** boîte de dialogue, cliquez sur **fermer**.  
  
8.  Fermez la fenêtre **Connexions réseau**.  
  
## <a name="ConfigRouting"></a>Configurer le routage entre sous-réseaux  
  
1.  Sur le **Démarrer** , tapez**cmd.exe**, avec le bouton droit **cmd**, cliquez sur **avancé**, puis cliquez sur **d’identification administrateur**. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la fenêtre d’invite de commandes, entrez les commandes suivantes. Après avoir entré chaque commande, appuyez sur ENTRÉE.  
  
    ```  
    netsh interface IPv4 add route 10.2.0.0/24 Corpnet 10.0.0.254  
    netsh interface IPv6 add route 2001:db8:2::/64 Corpnet 2001:db8:1::fe  
    ```  
  
3.  Fermez la fenêtre d’invite de commandes.  
  



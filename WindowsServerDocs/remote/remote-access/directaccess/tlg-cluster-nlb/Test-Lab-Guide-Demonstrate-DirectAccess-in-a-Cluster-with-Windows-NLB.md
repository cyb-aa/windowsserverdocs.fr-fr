---
title: Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB
description: Cette rubrique fait partie du Guide de laboratoire de test-démonstration de DirectAccess dans un cluster avec Windows NLB pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: db15dcf5-4d64-48d7-818a-06c2839e1289
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e0c82f9f56ea680c11cd612e17326fe7cf96aeca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388430"
---
# <a name="test-lab-guide-demonstrate-directaccess-in-a-cluster-with-windows-nlb"></a>Guide de laboratoire de test : démonstration de DirectAccess dans un cluster avec équilibrage de charge réseau Windows

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

L’accès à distance est un rôle serveur dans les systèmes d’exploitation Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012 qui permet aux utilisateurs distants d’accéder en toute sécurité aux ressources réseau internes à l’aide de DirectAccess ou du VPN RRAS. Ce guide contient des instructions pas à pas permettant d’élargir l’utilisation du [Guide de laboratoire de test : Démonstration de configuration d’un serveur DirectAccess unique dans un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004) dans le but de démontrer l’équilibrage de la charge réseau et la configuration de cluster DirectAccess.  
  
## <a name="about-this-guide"></a>À propos de ce guide  
Ce guide contient des instructions permettant de configurer le rôle serveur Accès à distance et d’en faire la démonstration à l’aide de six serveurs et deux ordinateurs clients. Le laboratoire de test Accès à distance réalisé avec équilibrage de la charge réseau simule un intranet, Internet, ainsi qu’un réseau domestique. Il fait la démonstration de la fonctionnalité Accès à distance dans différents scénarios de connexion Internet.  
  
> [!IMPORTANT]  
> Pour appuyer cette démonstration, il utilise un nombre minimum d’ordinateurs. La configuration détaillée dans ce guide est fournie à des fins de tests en laboratoire seulement et ne doit pas être utilisée dans un environnement de production.  
  
## <a name="KnownIssues"></a>Problèmes connus  
Les problèmes décrits ci-après sont connus et surviennent souvent lors de la configuration d’un scénario de cluster :  
  
-   Après avoir configuré DirectAccess dans un déploiement IPv4 uniquement avec une seule carte réseau et après avoir configuré automatiquement le DNS64 par défaut (l’adresse IPv6 qui contient « : 3333:: ») sur la carte réseau, la tentative d’activation de l’équilibrage de charge via la console Gestion de l’accès à distance entraîne l’affichage d’une invite demandant à l’utilisateur de fournir une adresse IPv6 DIP. Si une adresse IPv6 DIP est fournie, la configuration échoue après avoir cliqué sur **Valider** avec l’erreur : Le paramètre est incorrect.  
  
    Pour résoudre ce problème :  
  
    1.  Téléchargez la sauvegarde et restaurez les scripts à partir de [Sauvegarder et restaurer la configuration de l’accès à distance](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).  
  
    2.  Sauvegarder vos objets de stratégie de groupe à l’aide du script téléchargé Backup-RemoteAccess.ps1  
  
    3.  Essayez d’activer l’équilibrage de charge jusqu’à l’étape ayant provoqué l’échec. Dans la boîte de dialogue Activer l’équilibrage de charge, développez la zone des détails, cliquez avec le bouton droit dans cette dernière et cliquez sur **Copier le script**.  
  
    4.  Ouvrez le bloc-notes et collez le contenu du Presse-papiers. Par exemple :  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0','fdc4:29bd:abde:3333::2/128') -InternetVirtualIPAddress @('fdc4:29bd:abde:3333::1/128', '10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    5.  Fermez les boîtes de dialogue Accès à distance ouvertes et fermez la console de gestion de l’accès à distance.  
  
    6.  Modifiez le texte collé et supprimez les adresses IPv6. Par exemple :  
  
        ```  
        Set-RemoteAccessLoadBalancer -InternetDedicatedIPAddress @('10.244.4.19/255.255.255.0') -InternetVirtualIPAddress @('10.244.4.21/255.255.255.0') -ComputerName 'DA1.domain1.corp.contoso.com' -Verbose  
        ```  
  
    7.  Dans une fenêtre PowerShell avec élévation de privilèges, exécutez la commande à partir de l’étape précédente.  
  
    8.  Si l’applet de commande échoue pendant son exécution (pas en raison de valeurs d’entrée incorrectes), exécutez la commande Restore-RemoteAccess.ps1 et suivez les instructions pour vous assurer que l’intégrité de la configuration d’origine est maintenue.  
  
    9. Vous pouvez désormais ouvrir de nouveau la console de gestion de l’accès à distance.  
  



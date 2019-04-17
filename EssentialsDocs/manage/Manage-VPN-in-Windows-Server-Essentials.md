---
title: "Gérer les VPN dans Windows Server Essentials"
description: "Décrit comment utiliser WindowsServerEssentials"
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cc2b264a-b9a8-4114-9f7b-8604f77096e5
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 08a08a13d696371420bdfdf89f54320c787636b0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-vpn-in-windows-server-essentials"></a>Gérer les VPN dans Windows Server Essentials

>S’applique à: Windows Server2016Essentials, Windows Server2012R2 Essentials, Windows Server2012Essentials 
  
 Connexions de réseau privé virtuel (VPN) permettent aux utilisateurs qui travaillent à domicile ou êtes en déplacement d’accéder à un serveur sur un réseau privé à l’aide de l’infrastructure fournie par un réseau public, tel qu’Internet. Pour utiliser VPN pour accéder aux ressources du serveur, vous devez procéder comme suit:  
  
-   [Activer VPN pour l’accès à distance sur le serveur](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_1)  
  
-   [Définir des autorisations pour utilisateurs du réseau VPN](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_2)  
  
-   [Connecter des ordinateurs clients au serveur](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_Connect)  
  
-   [Utiliser VPN pour se connecter à Windows Server Essentials](Manage-VPN-in-Windows-Server-Essentials.md#BKMK_3)  
  
##  <a name="BKMK_1"></a>Activer VPN pour l’accès à distance sur le serveur  
 Effectuez la procédure suivante pour configurer un VPN dans Windows Server Essentials pour activer l’accès à distance.  
  
#### <a name="to-enable-vpn-in-windows-server-essentials"></a>Pour activer un VPN dans Windows Server Essentials  
  
1.  Ouvrez le tableau de bord.  
  
2.  Cliquez sur **paramètres**, puis cliquez sur le **accès en tout lieu** onglet.  
  
3.  Cliquez sur **configurer**. L’Assistant Configuration de n’importe quel endroit accès s’affiche.  
  
4.  Sur le **fonctionnalités choisir accès en tout lieu à activer** page, sélectionnez le **réseau privé virtuel** case à cocher.  
  
5.  Suivez les instructions pour terminer l’Assistant.  
  
##  <a name="BKMK_2"></a>Définir des autorisations pour utilisateurs du réseau VPN  
 Vous pouvez utiliser VPN pour se connecter à Windows Server Essentials et accéder à toutes les ressources qui sont stockés sur le serveur. Cela est particulièrement utile si vous disposez d’un ordinateur client qui est configuré avec des comptes de réseau qui peuvent être utilisés pour se connecter à un serveur Windows Server Essentials hébergé via une connexion VPN. Tous les comptes d’utilisateur récemment créé sur le serveur Windows Server Essentials hébergé doivent utiliser VPN pour se connecter à l’ordinateur client pour la première fois.  
  
#### <a name="to-set-vpn-permissions-for-network-users"></a>Pour définir les autorisations VPN des utilisateurs du réseau  
  
1.  Ouvrez le tableau de bord.  
  
2.  Dans la barre de navigation, cliquez sur **utilisateurs**.  
  
3.  Dans la liste des comptes d’utilisateur, sélectionnez le compte d’utilisateur que vous voulez accorder des autorisations pour accéder au Bureau à distance.  
  
4.  Dans le **< utilisateur\ > tâches** volet, cliquez sur **propriétés**.  
  
5.  Dans le **< utilisateur\ > Propriétés**, cliquez sur le **accès en tout lieu** onglet.  
  
6.  Sur le **accès en tout lieu** onglet, pour autoriser un utilisateur à se connecter au serveur à l’aide de VPN, sélectionnez le **autoriser privé réseau virtuel (VPN)** case à cocher.  
  
7.  Cliquez sur **appliquer**, puis cliquez sur **OK**.  
  
##  <a name="BKMK_Connect"></a>Connecter des ordinateurs clients au serveur  
 Une fois que VPN est activé sur un serveur exécutant Windows Server Essentials pour l’accès à distance, vous pouvez utiliser une connexion VPN pour se connecter à et accéder à toutes vos ressources qui sont stockés sur le serveur. Toutefois, vous devez d’abord connecter l’ordinateur au serveur. Lorsque vous vous connectez un ordinateur au serveur à l’aide de connecter un ordinateur à l’Assistant de serveur, une connexion réseau VPN est automatiquement générée sur l’ordinateur client et peut être utilisée pour accéder aux ressources serveur alors que l’utilisation à domicile ou êtes en déplacement. Pour obtenir des instructions détaillées sur la connexion de votre ordinateur au serveur, consultez [connecter des ordinateurs au serveur](../use/Get-Connected-in-Windows-Server-Essentials.md#BKMK_9).  
  
##  <a name="BKMK_3"></a>Utiliser VPN pour se connecter à Windows Server Essentials  
 Si vous disposez d’un ordinateur client qui est configuré avec des comptes de réseau qui peuvent être utilisés pour se connecter à un serveur hébergé exécutant Windows Server Essentials via une connexion VPN, tous les comptes d’utilisateur récemment créé le serveur hébergé doit utiliser VPN pour se connecter à l’ordinateur client pour la première fois. Effectuez la procédure suivante à partir de l’ordinateur client qui est connecté au serveur.  
  
#### <a name="to-use-vpn-to-remotely-access-server-resources"></a>Pour utiliser VPN pour accéder à distance aux ressources du serveur  
  
1.  Appuyez sur Ctrl + Alt + Suppr sur l’ordinateur client.  
  
2.  Cliquez sur **changer d’utilisateur** sur l’écran d’ouverture de session.  
  
3.  Cliquez sur l’icône d’ouverture de session réseau sur le coin inférieur droit de l’écran.  
  
4.  Connectez-vous au réseau Windows Server Essentials à l’aide de votre nom d’utilisateur réseau et le mot de passe.  
  
## <a name="see-also"></a>Voir aussi  
  
-   [Travailler à distance](../use/Work-Remotely-in-Windows-Server-Essentials.md)  
  
-   [Gérer l’accès en tout lieu](Manage-Anywhere-Access-in-Windows-Server-Essentials.md)  
  
-   [Gérer WindowsServerEssentials](Manage-Windows-Server-Essentials.md)

---
title: ÉTAPE 2 installer et configurer ROUTEUR1
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c6bff2acc15b7ff90731e0113ae0d5a429c635c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404800"
---
# <a name="step-2-install-and-configure-router1"></a>ÉTAPE 2 installer et configurer ROUTEUR1

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans ce guide de laboratoire de test multisite, l’ordinateur routeur fournit un pont IPv4 et IPv6 entre les sous-réseaux corpnet et 2-corpnet, et joue le rôle de routeur pour le trafic IP-HTTPs et Teredo.  
  
- Installer le système d’exploitation sur ROUTEUR1 
  
- Configurer les propriétés TCP/IP et renommer l’ordinateur  
  
- Désactiver le pare-feu
  
- Configurer le routage et le transfert
  
## <a name="install-the-operating-system-on-router1"></a>Installer le système d’exploitation sur ROUTEUR1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Pour installer le système d’exploitation sur ROUTEUR1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète).  
  
2.  Suivez les instructions pour effectuer l’installation, en spécifiant un mot de passe fort pour le compte Administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connectez ROUTEUR1 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez ROUTEUR1 aux sous-réseaux corpnet et 2-Corpnet.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurer les propriétés TCP/IP et renommer l’ordinateur  
Configurez les paramètres TCP/IP sur le routeur et renommez l’ordinateur en ROUTEUR1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Pour configurer les propriétés TCP/IP et renommer l’ordinateur  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur la carte réseau connectée à corpnet, cliquez sur **Renommer**, tapez **CORPNET**, puis appuyez sur entrée.  
  
3.  Cliquez avec le bouton droit sur **corpnet**, puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
5.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.0.0.254**. Dans **masque de sous-réseau**, tapez **255.255.255.0**, puis cliquez sur **OK**.  
  
6.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
7.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:1 :: Fe**. Dans **longueur du préfixe du sous-réseau**, tapez **64**, puis cliquez sur **OK**.  
  
8.  Dans la boîte de dialogue **Propriétés de corpnet** , cliquez sur **Fermer**.  
  
9. Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur la carte réseau qui est connectée à 2-corpnet, cliquez sur **Renommer**, tapez **2-CORPNET**, puis appuyez sur entrée.  
  
10. Cliquez avec le bouton droit sur **2-corpnet**, puis cliquez sur **Propriétés**.  
  
11. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
12. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.2.0.254**. Dans **masque de sous-réseau**, tapez **255.255.255.0**, puis cliquez sur **OK**.  
  
13. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
14. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:2 :: Fe**. Dans **longueur du préfixe du sous-réseau**, tapez **64**, puis cliquez sur **OK**.  
  
15. Dans la boîte de dialogue **Propriétés de 2-corpnet,** cliquez sur **Fermer**.  
  
16. Fermez la fenêtre **Connexions réseau**.  
  
17. Dans la console Gestionnaire de serveur, dans **serveur local**, dans la zone **Propriétés** , en regard de nom de l' **ordinateur**, cliquez sur le lien.  
  
18. Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
19. Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, dans nom de l' **ordinateur**, tapez **ROUTEUR1**, puis cliquez sur **OK**.  
  
20. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
21. Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
22. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
23. Une fois que l’ordinateur a redémarré, connectez-vous avec le compte d’administrateur local.  
  
## <a name="turn-off-the-firewall"></a>Désactiver le pare-feu  
Cet ordinateur est configuré uniquement pour assurer le routage entre les sous-réseaux corpnet et 2-Corpnet. par conséquent, le pare-feu doit être désactivé.  
  
### <a name="to-turn-off-the-firewall"></a>Pour désactiver le pare-feu  
  
1.  Dans l’écran d' **Accueil** , tapez**WF. msc**, puis appuyez sur entrée.  
  
2.  Dans pare-feu Windows avec fonctions avancées de sécurité, dans le volet **actions** , cliquez sur **Propriétés**.  
  
3.  Dans la boîte de dialogue **pare-feu Windows avec fonctions avancées de sécurité** , sous l’onglet **profil de domaine** , dans **État du pare-feu**, cliquez sur **désactivé**.  
  
4.  Dans la boîte de dialogue **pare-feu Windows avec fonctions avancées de sécurité** , sous l’onglet **profil privé** , dans **État du pare-feu**, cliquez sur **désactivé**.  
  
5.  Dans la boîte de dialogue **pare-feu Windows avec fonctions avancées de sécurité** , sous l’onglet **profil public** , dans **État du pare-feu**, cliquez sur **désactivé**, puis sur **OK**.  
  
6.  Fermez pare-feu Windows avec fonctions avancées de sécurité.  
  
## <a name="configure-routing-and-forwarding"></a>Configurer le routage et le transfert  
Pour fournir des services de routage et de transfert entre les sous-réseaux corpnet et 2-corpnet, vous devez activer le transfert sur les interfaces réseau et configurer des itinéraires statiques entre les sous-réseaux.  
  
### <a name="to-configure-static-routes"></a>Pour configurer des itinéraires statiques  
  
1.  Dans l’écran d' **Accueil** , tapez**cmd. exe**, puis appuyez sur entrée.  
  
2.  Activez le transfert sur les interfaces IPv4 et IPv6 des deux cartes réseau à l’aide des commandes suivantes. Après avoir entré chaque commande, appuyez sur entrée.  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  Activez le routage IP-HTTPs entre les sous-réseaux corpnet et 2-Corpnet.  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  Activez le routage Teredo entre les sous-réseaux corpnet et 2-Corpnet.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Fermez la fenêtre d’invite de commandes.

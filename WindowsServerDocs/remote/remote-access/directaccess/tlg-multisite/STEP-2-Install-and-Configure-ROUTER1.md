---
title: ÉTAPE 2 installer et configurer ROUTEUR1
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc20b1a0-540d-4531-a176-50b87c071600
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7d215ca234d63e7e393fbbce4d65e0803f023487
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283215"
---
# <a name="step-2-install-and-configure-router1"></a>ÉTAPE 2 installer et configurer ROUTEUR1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans ce guide de laboratoire de test multisite, l’ordinateur routeur fournit un pont IPv4 et IPv6 entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise et agit comme un routeur pour IP-HTTPS et le trafic Teredo.  
  
- Installer le système d’exploitation sur ROUTEUR1 
  
- Configurez les propriétés TCP/IP et renommer l’ordinateur  
  
- Désactiver le pare-feu
  
- Configurer le routage et de transfert
  
## <a name="install-the-operating-system-on-router1"></a>Installer le système d’exploitation sur ROUTEUR1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
### <a name="to-install-the-operating-system-on-router1"></a>Pour installer le système d’exploitation sur ROUTEUR1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète).  
  
2.  Suivez les instructions pour effectuer l’installation, en spécifiant un mot de passe fort pour le compte Administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter ROUTEUR1 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à ROUTEUR1 vers les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise.  
  
## <a name="configure-tcpip-properties-and-rename-the-computer"></a>Configurez les propriétés TCP/IP et renommer l’ordinateur  
Configurer les paramètres TCP/IP sur le routeur et renommez l’ordinateur ROUTEUR1.  
  
### <a name="to-configure-tcpip-properties-and-rename-the-computer"></a>Pour configurer les propriétés de TCP/IP et de renommer l’ordinateur  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans le **connexions réseau** fenêtre, cliquez sur la carte réseau qui est connectée au réseau d’entreprise, cliquez sur **renommer**, type **Corpnet**, puis appuyez sur ENTRÉE.  
  
3.  Avec le bouton droit **Corpnet**, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
5.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.0.0.254**. Dans **masque de sous-réseau**, type **255.255.255.0**, puis cliquez sur **OK**.  
  
6.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
7.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:1::fe**. Dans **longueur de préfixe de sous-réseau**, type **64**, puis cliquez sur **OK**.  
  
8.  Sur le **Corpnet propriétés** boîte dialogue, cliquez sur **fermer**.  
  
9. Dans le **connexions réseau** fenêtre, cliquez sur la carte réseau qui est connectée au réseau d’entreprise de 2, cliquez sur **renommer**, type **2-réseau d’entreprise**, puis appuyez sur ENTRÉE.  
  
10. Avec le bouton droit **2-réseau d’entreprise**, puis cliquez sur **propriétés**.  
  
11. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
12. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.2.0.254**. Dans **masque de sous-réseau**, type **255.255.255.0**, puis cliquez sur **OK**.  
  
13. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
14. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:2::fe**. Dans **longueur de préfixe de sous-réseau**, type **64**, puis cliquez sur **OK**.  
  
15. Sur le **2-réseau d’entreprise propriétés** boîte dialogue, cliquez sur **fermer**.  
  
16. Fermez la fenêtre **Connexions réseau**.  
  
17. Dans la console Gestionnaire de serveur, dans **serveur Local**, dans le **propriétés** zone, en regard **nom de l’ordinateur**, cliquez sur le lien.  
  
18. Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
19. Sur le **modification du nom ou du domaine d’ordinateur** boîte de dialogue **nom de l’ordinateur**, type **ROUTEUR1**, puis cliquez sur **OK**.  
  
20. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
21. Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
22. Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
23. Une fois que l’ordinateur a redémarré, connectez-vous avec le compte d’administrateur local.  
  
## <a name="turn-off-the-firewall"></a>Désactiver le pare-feu  
Cet ordinateur est configuré uniquement pour fournir un routage entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise ; Par conséquent, le pare-feu doit être désactivé.  
  
### <a name="to-turn-off-the-firewall"></a>Pour désactiver le pare-feu  
  
1.  Sur le **Démarrer** , tapez**wf.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le pare-feu Windows avec fonctions avancées de sécurité, dans le **Actions** volet, cliquez sur **propriétés**.  
  
3.  Sur le **pare-feu Windows avec fonctions avancées de sécurité** boîte de dialogue le **profil de domaine** sous l’onglet **état du pare-feu**, cliquez sur **hors**.  
  
4.  Sur le **pare-feu Windows avec fonctions avancées de sécurité** boîte de dialogue le **profil privé** sous l’onglet **état du pare-feu**, cliquez sur **hors**.  
  
5.  Sur le **pare-feu Windows avec fonctions avancées de sécurité** boîte de dialogue le **profil Public** sous l’onglet **état du pare-feu**, cliquez sur **hors**, puis Cliquez sur **OK**.  
  
6.  Fermez les pare-feu Windows avec fonctions avancées de sécurité.  
  
## <a name="configure-routing-and-forwarding"></a>Configurer le routage et de transfert  
Pour fournir le routage et de transfert des services entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise, vous devez activer le transfert sur les interfaces réseau et configurer des itinéraires statiques entre les sous-réseaux.  
  
### <a name="to-configure-static-routes"></a>Pour configurer des itinéraires statiques  
  
1.  Sur le **Démarrer** , tapez**cmd.exe**, puis appuyez sur ENTRÉE.  
  
2.  Activer le transfert sur les interfaces IPv4 et IPv6 de deux cartes réseau à l’aide des commandes suivantes. Après avoir entré chaque commande, appuyez sur ENTRÉE.  
  
    ```  
    netsh interface IPv4 set interface Corpnet forwarding=enabled  
    netsh interface IPv4 set interface 2-Corpnet forwarding=enabled  
    netsh interface IPv6 set interface Corpnet forwarding=enabled  
    netsh interface IPv6 set interface 2-Corpnet forwarding=enabled  
    ```  
  
3.  Activer le routage IP-HTTPS entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise.  
  
    ```  
    netsh interface IPv6 add route 2001:db8:1:1000::/59 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:db8:2:2000::/59 2-Corpnet 2001:db8:2::20  
    ```  
  
4.  Activer le routage de Teredo entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise.  
  
    ```  
    netsh interface IPv6 add route 2001:0:836b:2::/64 Corpnet 2001:db8:1::2  
    netsh interface IPv6 add route 2001:0:836b:14::/64 2-Corpnet 2001:db8:2::20  
    ```  
  
5.  Fermez la fenêtre d’invite de commandes.

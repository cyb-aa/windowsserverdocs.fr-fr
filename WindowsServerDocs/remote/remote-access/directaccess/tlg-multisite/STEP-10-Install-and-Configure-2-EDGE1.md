---
title: 'ÉTAPE 10 : installer et configurer 2-EDGE1'
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 7d21d80f4970a501e31a053483c37268bdddb811
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314613"
---
# <a name="step-10-install-and-configure-2-edge1"></a>ÉTAPE 10 : installer et configurer 2-EDGE1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

2-la configuration de EDGE1 est constituée des éléments suivants :  
  
- Installez le système d’exploitation sur 2 EDGE1. Installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 sur 2 EDGE1.  
  
- Configurer les propriétés TCP/IP. Configurez 2-EDGE1 avec des adresses statiques sur les deux interfaces réseau.  
  
- Configurer le routage entre les sous-réseaux. Pour activer la communication entre les sous-réseaux corpnet et 2-corpnet, vous devez configurer le routage.  
  
- Joindre 2-EDGE1 au domaine CORP2. Joindre 2-EDGE1 au domaine corp2.corp.contoso.com.  
  
- Obtenez des certificats sur 2 EDGE1. Des certificats sont requis pour la connexion IPsec entre les clients DirectAccess et le serveur d’accès à distance, et pour authentifier l’écouteur IP-HTTPs lorsque les clients se connectent via HTTPs.  
  
- Fournir l’accès à CORP\User1. L’utilisateur Corp\user1. est l’administrateur de l’accès à distance. Pour permettre à cet utilisateur d’apporter des modifications à 2 EDGE1 à partir de EDGE1, vous devez accorder l’accès à l’utilisateur.  
  
- Installez le rôle accès à distance sur 2 EDGE1. Pour activer un déploiement multisite, vous devez installer le rôle accès à distance sur 2 EDGE1.  
  
2-EDGE1 doit avoir deux cartes réseau installées.  
  
## <a name="install-the-operating-system-on-2-edge1"></a><a name="installOS"></a>Installer le système d’exploitation sur 2-EDGE1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte administrateur local. Ouvrez une session en utilisant le compte d'administrateur local.  
  
3.  Connectez 2-EDGE1 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez une carte réseau au sous-réseau 2-corpnet et l’autre à l’Internet simulé.  
  
## <a name="configure-tcpip-properties"></a><a name="tcpip"></a>Configurer les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans **connexions réseau**, cliquez avec le bouton droit sur la connexion réseau qui est connectée au sous-réseau 2-corpnet, cliquez sur **Renommer**, tapez **2-CORPNET**, puis appuyez sur entrée.  
  
3.  Cliquez avec le bouton droit sur **2-corpnet**, puis cliquez sur **Propriétés**.  
  
4.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
5.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.2.0.20**, dans **masque de sous-réseau**, tapez **255.255.255.0**.  
  
6.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, tapez **10.2.0.1**, et dans **serveur DNS auxiliaire**, tapez **10.0.0.1**.  
  
7.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
8.  Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
9. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
10. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:2 :: 20**, dans **longueur du préfixe du sous-réseau**, tapez **64**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**et, dans **serveur DNS préféré**, tapez **2001 : DB8:2 :: 1**, dans **serveur DNS auxiliaire**, tapez **2001 : DB8:1 :: 1**.  
  
11. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
12. Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
13. Dans la boîte de dialogue **Propriétés de 2-corpnet** , cliquez sur **Fermer**.  
  
14. Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur la connexion réseau qui est connectée au sous-réseau Internet, cliquez sur **Renommer**, tapez **Internet**, puis appuyez sur entrée.  
  
15. Cliquez avec le bouton droit sur **Internet**, puis cliquez sur **Propriétés**.  
  
16. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
17. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **131.107.0.20**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**.  
  
18. Cliquez sur **Avancé**. Dans l’onglet **Paramètres IP**, dans la zone **Adresses IP**, cliquez sur **Ajouter**. Dans la boîte de dialogue **adresse TCP/IP** , dans **adresse IP** , tapez **131.107.0.21**, dans **masque de sous-réseau** , tapez **255.255.255.0**, puis cliquez sur **Ajouter**.  
  
19. Cliquez sur l’onglet **DNS**.  
  
20. Dans **suffixe DNS pour cette connexion**, tapez **ISP.example.com**, cliquez deux fois sur **OK** , puis cliquez sur **Fermer**.  
  
21. Fermez la fenêtre **Connexions réseau**.  
  
## <a name="configure-routing-between-subnets"></a><a name="routing"></a>Configurer le routage entre les sous-réseaux  
  
1.  Dans l’écran d' **Accueil** , tapez**cmd. exe**, puis appuyez sur entrée.  
  
2.  Dans la fenêtre d’invite de commandes, entrez les commandes suivantes. Après avoir entré chaque commande, appuyez sur entrée.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Pour vérifier la communication réseau entre 2-EDGE1 et DC1, tapez **ping dc1.Corp.contoso.com**.  
  
4.  Vérifiez qu’il y a quatre réponses provenant de l’adresse IPv4, 10.0.0.1 ou de l’adresse IPv6, 2001 : DB8:1 :: 1.  
  
5.  Fermez la fenêtre d'invite de commandes.  
  
## <a name="join-2-edge1-to-the-corp2-domain"></a><a name="Join"></a>Joindre 2-EDGE1 au domaine CORP2  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur local**, dans la zone **Propriétés** , en regard de nom de l' **ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Dans la boîte de dialogue **modification du nom ou du domaine** de l’ordinateur, dans nom de l' **ordinateur**, tapez **2-Edge1**. Dans **membre de**, cliquez sur **domaine**, tapez **Corp2.Corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Si vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez **administrateur** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Quand vous voyez une boîte de dialogue qui vous permet de vous accueillir dans le domaine corp2.corp.contoso.com, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Une fois l’ordinateur redémarré, cliquez sur **changer d’utilisateur**, puis sur **autre utilisateur** et connectez-vous au domaine CORP2 avec le compte administrateur.  
  
## <a name="obtain-certificates-on-2-edge1"></a><a name="certs"></a>Obtenir des certificats sur 2 EDGE1  
  
1.  Dans l’écran **Démarrer** , tapez**MMC. exe**, puis appuyez sur entrée.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l’arborescence de la console du composant logiciel enfichable Certificats, ouvrez **certificats (ordinateur local) \Personal**.  
  
5.  Cliquez avec le bouton droit sur **personnel**, pointez sur **toutes les tâches**, puis cliquez sur **demander un nouveau certificat**.  
  
6.  Cliquez deux fois sur **Suivant**.  
  
7.  Dans la **page demander des certificats** , activez les cases à cocher **authentification client-serveur** et **serveur Web** , puis cliquez sur l' **inscription pour obtenir ce certificat nécessite des informations supplémentaires**.  
  
8.  Dans la boîte de dialogue **Propriétés du certificat** , sous l’onglet **sujet** , dans la zone nom de l' **objet** , dans **type**, sélectionnez **nom commun**.  
  
9. Dans **valeur**, tapez **2-Edge1.contoso.com**, puis cliquez sur **Ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans **valeur**, entrez **2-Edge1.contoso.com**, puis cliquez sur **Ajouter**.  
  
12. Sous l’onglet **général** , dans **nom convivial**, tapez **certificat IP-HTTPS**.  
  
13. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
14. Dans le volet d’informations du composant logiciel enfichable Certificats, vérifiez qu’un nouveau certificat portant le nom 2-edge1.contoso.com a été inscrit avec des rôles prévus pour l’authentification du serveur et qu’un nouveau certificat portant le nom 2-edge1.corp2.corp.contoso.com a été inscrit auprès de Rôles prévus pour l’authentification du client et l’authentification du serveur.  
  
15. Fermez la fenêtre de console. Si vous êtes invité à enregistrer les paramètres, cliquez sur **non**.  
  
## <a name="provide-access-to-corpuser1"></a><a name="Access"></a>Fournir l’accès à Corp\user1.  
  
1.  Dans l’écran d' **Accueil** , tapez**compmgmt. msc**, puis appuyez sur entrée.  
  
2.  Dans le volet gauche, cliquez sur **utilisateurs et groupes locaux**.  
  
3.  Double-cliquez sur **groupes**, puis double-cliquez sur **administrateurs**.  
  
4.  Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **Ajouter**, puis dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** , cliquez sur **emplacements**.  
  
5.  Dans la boîte de dialogue **emplacements** , dans l’arborescence **emplacement** , cliquez sur **Corp.contoso.com**, puis sur **OK**.  
  
6.  Dans la **zone Entrez les noms des objets à sélectionner** , tapez **User1**, puis cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés des administrateurs** , cliquez sur **OK**.  
  
8.  Fermez la fenêtre Gestion de l'ordinateur.  
  
## <a name="install-the-remote-access-role-on-2-edge1"></a><a name="InstallDA"></a>Installer le rôle accès à distance sur 2-EDGE1  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le**Sélectionner des rôles de serveur**boîte de dialogue, sélectionnez**accès distant**cliquez sur**Ajouter des fonctionnalités**puis cliquez sur**Suivant**.  
  
4.  Cliquez sur **Suivant** cinq fois.  
  
5.  Dans la page **Confirmer les sélections d'installation**, cliquez sur **Installer**.  
  
6.  Dans la boîte de dialogue **Progression de l'installation**, vérifiez que l'installation a réussi, puis cliquez sur **Fermer**.  
  



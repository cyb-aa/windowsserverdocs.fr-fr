---
title: ÉTAPE 10 installer et configurer 2-EDGE1
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
ms.assetid: d98d6f7a-a2e6-45b1-9c63-08e2986a5c03
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1640dbae52a1a7c93355b34822d72faa5351bcda
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860310"
---
# <a name="step-10-install-and-configure-2-edge1"></a>ÉTAPE 10 installer et configurer 2-EDGE1

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

2-EDGE1 configuration se compose des éléments suivants :  
  
- Installer le système d’exploitation sur 2-EDGE1. Installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 sur 2-EDGE1.  
  
- Configurer les propriétés TCP/IP. Configurer 2-EDGE1 avec des adresses statiques sur les deux interfaces réseau.  
  
- Configurer le routage entre sous-réseaux. Pour permettre la communication entre les sous-réseaux du réseau d’entreprise et 2-réseau d’entreprise, vous devez configurer le routage.  
  
- Joindre 2-EDGE1 au domaine CORP2. Joindre 2-EDGE1 au domaine corp2.corp.contoso.com.  
  
- Obtenir des certificats sur 2-EDGE1. Les certificats sont requis pour la connexion IPsec entre les clients DirectAccess et le serveur d’accès à distance et d’authentifier l’écouteur IP-HTTPS lorsque les clients se connectent via le protocole HTTPS.  
  
- Fournir l’accès à CORP\User1. L’utilisateur CORP\User1 est l’administrateur de l’accès à distance. Pour activer cet utilisateur d’apporter des modifications à 2-EDGE1 à partir de EDGE1, vous devez accorder l’accès à l’utilisateur.  
  
- Installez le rôle accès à distance sur 2-EDGE1. Pour activer un déploiement multisite, vous devez installer le rôle accès à distance sur 2-EDGE1.  
  
2-EDGE1 doit avoir deux cartes réseau installées.  
  
## <a name="installOS"></a>Installer le système d’exploitation sur 2-EDGE1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
2.  Suivez les instructions pour terminer l’installation, en spécifiant Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète) et un mot de passe fort pour le compte d’administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter 2-EDGE1 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à une carte réseau au sous-réseau Corpnet de 2 et l’autre pour l’Internet simulé.  
  
## <a name="tcpip"></a>Configurez les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans **connexions réseau**, avec le bouton droit de la connexion réseau qui est connectée au sous-réseau Corpnet de 2, cliquez sur **renommer**, type **2-réseau d’entreprise**, puis appuyez sur ENTRÉE.  
  
3.  Avec le bouton droit **2-réseau d’entreprise**, puis cliquez sur **propriétés**.  
  
4.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
5.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.2.0.20**, dans **masque de sous-réseau**, type **255.255.255.0**.  
  
6.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, type **10.2.0.1**et dans **serveur DNS auxiliaire**, type **10.0.0.1**.  
  
7.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
8.  Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
9. Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)**, puis cliquez sur **Propriétés**.  
  
10. Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:2::20**, dans **longueur de préfixe de sous-réseau**, type **64**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**et dans **serveur DNS préféré**, type **2001:db8:2::1**, dans **serveur DNS auxiliaire**, type **2001:db8:1::1**.  
  
11. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
12. Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
13. Sur le **2-réseau d’entreprise propriétés** boîte de dialogue, cliquez sur **fermer**.  
  
14. Dans le **connexions réseau** fenêtre, cliquez sur la connexion réseau qui est connectée au sous-réseau Internet, cliquez sur **renommer**, type **Internet**, puis appuyez sur entrée .  
  
15. Cliquez avec le bouton droit sur **Internet**, puis cliquez sur **Propriétés**.  
  
16. Cliquez sur **Protocole Internet version 4 (TCP/IPv4)**, puis sur **Propriétés**.  
  
17. Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **131.107.0.20**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**.  
  
18. Cliquez sur **Avancé**. Dans l’onglet **Paramètres IP**, dans la zone **Adresses IP**, cliquez sur **Ajouter**. Sur le **adresse TCP/IP** boîte de dialogue **adresse IP** type **131.107.0.21**, dans **masque de sous-réseau** type **255.255.255.0** , puis cliquez sur **ajouter**.  
  
19. Cliquez sur l’onglet **DNS**.  
  
20. Dans **suffixe DNS pour cette connexion**, type **ISP.exemple.com**, cliquez sur **OK** à deux reprises, puis cliquez sur **fermer**.  
  
21. Fermez la fenêtre **Connexions réseau**.  
  
## <a name="routing"></a>Configurer le routage entre sous-réseaux  
  
1.  Sur le **Démarrer** , tapez**cmd.exe**, puis appuyez sur ENTRÉE.  
  
2.  Dans la fenêtre d’invite de commandes, entrez les commandes suivantes. Après avoir entré chaque commande, appuyez sur ENTRÉE.  
  
    ```  
    netsh interface IPv4 add route 10.0.0.0/24 2-Corpnet 10.2.0.254  
    netsh interface IPv6 add route 2001:db8:1::/64 2-Corpnet 2001:db8:2::fe  
    ```  
  
3.  Pour vérifier la communication réseau entre 2-EDGE1 et DC1, tapez **ping dc1.corp.contoso.com**.  
  
4.  Vérifiez qu’il existe quatre réponses depuis l’adresse IPv4, 10.0.0.1, ou à partir de l’adresse IPv6, 2001:db8:1::1.  
  
5.  Fermez la fenêtre d’invite de commandes.  
  
## <a name="Join"></a>Joindre des 2-EDGE1 au domaine CORP2  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur Local**, dans le **propriétés** zone, en regard **nom de l’ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Sur le **modification du nom ou du domaine d’ordinateur** boîte de dialogue **nom de l’ordinateur**, type **2-EDGE1**. Dans **membre**, cliquez sur **domaine**, type **corp2.corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Si vous êtes invité à un nom d’utilisateur et le mot de passe, tapez **administrateur** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Lorsque vous voyez une boîte de dialogue pour vous accueillir dans le domaine corp2.corp.contoso.com, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Une fois que l’ordinateur a redémarré, cliquez sur **changer d’utilisateur**, puis cliquez sur **autre utilisateur** et ouvrez une session sur le domaine CORP2 avec le compte administrateur.  
  
## <a name="certs"></a>Obtenir des certificats sur 2-EDGE1  
  
1.  Sur le **Démarrer** , tapez**mmc.exe**, puis appuyez sur ENTRÉE.  
  
2.  Dans la console MMC, dans le menu **Fichier**, cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3.  Dans la boîte de dialogue **Ajouter ou supprimer des composants logiciels enfichables**, cliquez sur **Certificats**, sur **Ajouter**, sur **Le compte de l'ordinateur**, sur **Suivant**, sur **Ordinateur local**, sur **Terminer**, puis sur **OK**.  
  
4.  Dans l’arborescence de la console du composant logiciel enfichable Certificats, ouvrez **certificats (ordinateur Local) \Personal**.  
  
5.  Avec le bouton droit **personnel**, pointez sur **toutes les tâches**, puis cliquez sur **demander un nouveau certificat**.  
  
6.  Cliquez sur **Suivant** deux fois.  
  
7.  Sur le **demander des certificats** page, sélectionnez le **Client-Server Authentication** et **serveur Web** cases à cocher, puis cliquez sur **plus d’informations inscription pour obtenir ce certificat nécessite des**.  
  
8.  Sur le **propriétés du certificat** boîte de dialogue le **sujet** sous l’onglet le **nom de l’objet** zone, dans **Type**, sélectionnez  **Nom commun**.  
  
9. Dans **valeur**, type **2-edge1.contoso.com**, puis cliquez sur **ajouter**.  
  
10. Dans la zone **Autre nom**, dans **Type**, sélectionnez **DNS**.  
  
11. Dans **valeur**, entrez **2-edge1.contoso.com**, puis cliquez sur **ajouter**.  
  
12. Sur le **général** sous l’onglet **nom convivial**, type **certificat IP-HTTPS**.  
  
13. Cliquez sur **OK**, sur **Inscrire**, puis sur **Terminer**.  
  
14. Dans le volet détails du composant logiciel enfichable Certificats, vérifiez qu’un nouveau certificat avec le nom 2-edge1.contoso.com a été inscrit avec rôles prévus l’à des fins d’authentification de serveur, et un nouveau certificat avec le nom 2-edge1.corp2.corp.contoso.com a été inscrit avec Rôles prévus de l’authentification du Client et l’authentification du serveur.  
  
15. Fermez la fenêtre de console. Si vous êtes invité à enregistrer les paramètres, cliquez sur **non**.  
  
## <a name="Access"></a>Fournir l’accès à CORP\User1  
  
1.  Sur le **Démarrer** , tapez**compmgmt.msc**, puis appuyez sur ENTRÉE.  
  
2.  Dans le volet gauche, cliquez sur **utilisateurs et groupes locaux**.  
  
3.  Double-cliquez sur **groupes**, puis double-cliquez sur **administrateurs**.  
  
4.  Sur le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **ajouter**, puis, dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue, cliquez sur  **Emplacements**.  
  
5.  Sur le **emplacements** boîte de dialogue le **emplacement** arborescence, cliquez sur **corp.contoso.com**, puis cliquez sur **OK**.  
  
6.  Dans le **Entrez les noms des objets à sélectionner** type **User1**, puis cliquez sur **OK**.  
  
7.  Sur le **propriétés du groupe Administrateurs** boîte de dialogue, cliquez sur **OK**.  
  
8.  Fermez la fenêtre de gestion de l’ordinateur.  
  
## <a name="InstallDA"></a>Installer le rôle de l’accès à distance sur 2-EDGE1  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **Suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur.  
  
3.  Sur le**Sélectionner des rôles de serveur**boîte de dialogue, sélectionnez**accès distant**cliquez sur**Ajouter des fonctionnalités**puis cliquez sur**Suivant**.  
  
4.  Cliquez sur **Suivant** cinq fois.  
  
5.  Dans la boîte de dialogue **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
6.  Dans la boîte de dialogue **Progression de l’installation** , vérifiez que l’installation s’est correctement déroulée et cliquez sur **Fermer**.  
  



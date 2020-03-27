---
title: 'ÉTAPE 7 : installer et configurer 2-APP1'
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fddffbc2954ef7f0687fc7865ec295295b32983a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314523"
---
# <a name="step-7-install-and-configure-2-app1"></a>ÉTAPE 7 : installer et configurer 2-APP1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

2-APP1 fournit des services de partage de fichiers et Web. 2-APP1 la configuration comprend les éléments suivants :  
  
- Installer le système d’exploitation sur 2-APP1  
  
- Configurer les propriétés TCP/IP  
  
- Joindre 2-APP1 au domaine CORP2  
  
- Installer le rôle serveur Web (IIS) sur 2-APP1  
  
- Créer un dossier partagé sur 2-APP1 
  
## <a name="install-the-operating-system-on-2-app1"></a><a name="bkmk_InstallOS"></a>Installer le système d’exploitation sur 2-APP1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Pour installer le système d’exploitation sur 2-APP1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète).  
  
2.  Suivez les instructions pour effectuer l’installation, en spécifiant un mot de passe fort pour le compte Administrateur local. Ouvrez une session en utilisant le compte d'administrateur local.  
  
3.  Connectez 2-APP1 à un réseau disposant d’un accès Internet et exécutez Windows Update pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012, puis vous déconnecter d’Internet.  
  
4.  Connectez 2-APP1 au sous-réseau 2-Corpnet.  
  
## <a name="configure-tcpip-properties"></a><a name="bkmk_TCP"></a>Configurer les propriétés TCP/IP  
Configurez les propriétés TCP/IP sur 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Pour configurer les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur local**, puis dans la zone **Propriétés** , en regard de **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans la fenêtre **connexions réseau** , cliquez avec le bouton droit sur **connexion Ethernet câblée**, puis cliquez sur **Propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, tapez **10.2.0.3**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, tapez **10.2.0.254**.  
  
5.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, tapez **10.2.0.1**.  
  
6.  Cliquez sur **avancé**, puis sur l’onglet **DNS** . Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
7.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
8.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, tapez **2001 : DB8:2 :: 3**. Dans **longueur du préfixe du sous-réseau**, tapez **64**. Dans **passerelle par défaut**, tapez **2001 : DB8:2 :: Fe**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**et, dans **serveur DNS préféré**, tapez **2001 : DB8:2 :: 1**.  
  
9. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
10. Dans **suffixe DNS pour cette connexion**, tapez **corp2.Corp.contoso.com**, puis cliquez deux fois sur **OK** .  
  
11. Dans la boîte de dialogue **Propriétés de connexion Ethernet câblée** , cliquez sur **Fermer**.  
  
12. Fermez la fenêtre **Connexions réseau**.  
  
## <a name="join-2-app1-to-the-corp2-domain"></a><a name="bkmk_JoinDomain"></a>Joindre 2-APP1 au domaine CORP2  
Joindre 2-APP1 au domaine corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Pour joindre 2-APP1 au domaine CORP2  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur local**, dans la zone **Propriétés** , en regard de nom de l' **ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Dans **nom**de l’ordinateur, tapez **2-App1**. Dans **membre de**, cliquez sur **domaine**, tapez **Corp2.Corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à entrer un nom d’utilisateur et un mot de passe, tapez **administrateur** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Quand vous voyez une boîte de dialogue qui vous permet de vous accueillir dans le domaine corp2.corp.contoso.com, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Une fois l’ordinateur redémarré, cliquez sur **changer d’utilisateur**, puis sur **autre utilisateur** et connectez-vous au domaine CORP2 avec le compte administrateur.  
  
## <a name="install-the-web-server-iis-role-on-2-app1"></a><a name="bkmk_IIS"></a>Installer le rôle serveur Web (IIS) sur 2-APP1  
Installez le rôle serveur Web (IIS) pour créer 2-APP1 un serveur Web.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Pour installer le rôle serveur Web (IIS)  
  
1.  Dans la console Gestionnaire de serveur, dans le **tableau de bord**, cliquez sur **Ajouter des rôles et des fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection du rôle de serveur  
  
3.  Dans la page **Sélectionner des rôles de serveurs** , sélectionnez **serveur Web (IIS)** , puis cliquez sur **suivant** quatre fois.  
  
4.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
5.  Vérifiez que l’installation s’est correctement déroulée, puis cliquez sur **Fermer**.  
  
## <a name="create-a-shared-folder-on-2-app1"></a><a name="bkmk_Share"></a>Créer un dossier partagé sur 2-APP1  
Créez un dossier partagé et un fichier texte dans le dossier sur 2-APP1.  
  
#### <a name="to-create-a-shared-folder"></a>Pour créer un dossier partagé  
  
1.  Dans l’écran d' **Accueil** , tapez**Explorer. exe**, puis appuyez sur entrée.  
  
2.  Cliquez sur **ordinateur**, puis double-cliquez sur **disque local (C :)** .  
  
3.  Cliquez sur **nouveau dossier**, tapez **Files**, puis appuyez sur entrée. Laissez la fenêtre du **disque local** ouverte.  
  
4.  Dans l’écran **Démarrer** , tapez**notepad. exe**, cliquez avec le bouton droit sur **bloc-notes**, cliquez sur **avancé**, puis sur **exécuter en tant qu’administrateur**.  
  
5.  Dans la fenêtre **sans titre-bloc-notes** , tapez **il s’agit d’un fichier partagé sur 2-App1**.  
  
6.  Cliquez sur **fichier**, sur **Enregistrer**, sur **ordinateur**, double-cliquez sur **disque local (C :)** , puis double-cliquez sur le dossier **fichiers** .  
  
7.  Dans **nom de fichier**, tapez **example. txt**, puis cliquez sur **Enregistrer**. Fermez le Bloc-notes.  
  
8.  Dans la fenêtre **disque local** , cliquez avec le bouton droit sur le dossier **fichiers** , pointez sur **partager avec**, puis cliquez sur **personnes spécifiques**.  
  
9. Dans la boîte de dialogue **partage de fichiers** , dans la liste déroulante, cliquez sur **tout le monde**, puis cliquez sur **Ajouter**. Dans **niveau d’autorisation** pour **tout le monde**, cliquez sur **lecture/écriture**.  
  
10. Cliquez sur **partager**, puis sur **terminé**.  
  
11. Fermez la fenêtre du **disque local** .  
  



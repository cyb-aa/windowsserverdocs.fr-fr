---
title: ÉTAPE 7 installer et configurer APP1-2
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1cc0abc6-be4d-4cbe-bd0c-cc448bf294f6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4746ff5118814506d20983d3570881366297322f
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283179"
---
# <a name="step-7-install-and-configure-2-app1"></a>ÉTAPE 7 installer et configurer APP1-2

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

2-APP1 fournit web et les services de partage de fichiers. Configuration d’APP1-2 se compose des éléments suivants :  
  
- Installer le système d’exploitation sur 2-APP1  
  
- Configurer les propriétés TCP/IP  
  
- Joindre des 2-APP1 au domaine CORP2  
  
- Installer le rôle de serveur Web (IIS) sur 2-APP1  
  
- Créez un dossier partagé sur 2-APP1 
  
## <a name="bkmk_InstallOS"></a>Installer le système d’exploitation sur 2-APP1  
Tout d’abord, installez Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
#### <a name="to-install-the-operating-system-on-2-app1"></a>Pour installer le système d’exploitation sur 2-APP1  
  
1.  Démarrez l’installation de Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 (installation complète).  
  
2.  Suivez les instructions pour effectuer l’installation, en spécifiant un mot de passe fort pour le compte Administrateur local. Ouvrez une session à l’aide du compte Administrateur local.  
  
3.  Connecter 2-APP1 à un réseau qui a accès à Internet et exécutez la mise à jour de Windows pour installer les dernières mises à jour pour Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 et vous déconnecter puis à partir d’Internet.  
  
4.  Se connecter à 2-APP1 au sous-réseau Corpnet de 2.  
  
## <a name="bkmk_TCP"></a>Configurez les propriétés TCP/IP  
Configurez les propriétés TCP/IP sur 2-APP1.  
  
#### <a name="to-configure-tcpip-properties"></a>Pour configurer les propriétés TCP/IP  
  
1.  Dans la console Gestionnaire de serveur, cliquez sur **serveur Local**, puis, dans le **propriétés** zone, en regard **connexion Ethernet câblée**, cliquez sur le lien.  
  
2.  Dans le **connexions réseau** fenêtre, avec le bouton droit **connexion Ethernet câblée**, puis cliquez sur **propriétés**.  
  
3.  Cliquez sur **Protocole Internet version 4 (TCP/IPv4)** , puis sur **Propriétés**.  
  
4.  Cliquez sur **Utiliser l’adresse IP suivante**. Dans **adresse IP**, type **10.2.0.3**. Dans la zone **Masque de sous-réseau**, tapez **255.255.255.0**. Dans **passerelle par défaut**, type **10.2.0.254**.  
  
5.  Cliquez sur **Utiliser l’adresse de serveur DNS suivante**. Dans **serveur DNS préféré**, type **10.2.0.1**.  
  
6.  Cliquez sur **Avancé**, puis sur l’onglet **DNS**. Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
7.  Cliquez sur **Internet Protocol Version 6 (TCP/IPv6)** , puis cliquez sur **Propriétés**.  
  
8.  Cliquez sur **utiliser l’adresse IPv6 suivante**. Dans **adresse IPv6**, type **2001:db8:2::3**. Dans **longueur de préfixe de sous-réseau**, type **64**. Dans **passerelle par défaut**, type **2001:db8:2::fe**. Cliquez sur **utiliser l’adresse de serveur DNS suivante**et dans **serveur DNS préféré**, type **2001:db8:2::1**.  
  
9. Cliquez sur **Avancé**, puis sur l’onglet **DNS**.  
  
10. Dans **suffixe DNS pour cette connexion**, type **corp2.corp.contoso.com**, puis cliquez sur **OK** à deux reprises.  
  
11. Sur le **propriétés de connexion Ethernet câblée** boîte dialogue, cliquez sur **fermer**.  
  
12. Fermez la fenêtre **Connexions réseau**.  
  
## <a name="bkmk_JoinDomain"></a>Joindre des 2-APP1 au domaine CORP2  
Joindre 2-APP1 au domaine corp2.corp.contoso.com.  
  
#### <a name="to-join-2-app1-to-the-corp2-domain"></a>Pour joindre des 2-APP1 au domaine CORP2  
  
1.  Dans la console Gestionnaire de serveur, dans **serveur Local**, dans le **propriétés** zone, en regard **nom de l’ordinateur**, cliquez sur le lien.  
  
2.  Dans la boîte de dialogue **Propriétés système**, sous l'onglet **Nom de l'ordinateur**, cliquez sur **Modifier**.  
  
3.  Dans **nom de l’ordinateur**, type **2-APP1**. Dans **membre**, cliquez sur **domaine**, type **corp2.corp.contoso.com**, puis cliquez sur **OK**.  
  
4.  Lorsque vous êtes invité à un nom d’utilisateur et le mot de passe, tapez **administrateur** et son mot de passe, puis cliquez sur **OK**.  
  
5.  Lorsque vous voyez une boîte de dialogue pour vous accueillir dans le domaine corp2.corp.contoso.com, cliquez sur **OK**.  
  
6.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **OK**.  
  
7.  Dans la boîte de dialogue **Propriétés système**, cliquez sur **Fermer**.  
  
8.  Lorsque vous êtes invité à redémarrer l’ordinateur, cliquez sur **Redémarrer maintenant**.  
  
9. Une fois l’ordinateur redémarré, cliquez sur **changer d’utilisateur**, puis cliquez sur **autre utilisateur** et ouvrez une session sur le domaine CORP2 avec le compte administrateur.  
  
## <a name="bkmk_IIS"></a>Installer le rôle de serveur Web (IIS) sur 2-APP1  
Installez le rôle de serveur Web (IIS) pour rendre 2-APP1 un serveur web.  
  
#### <a name="to-install-the-web-server-iis-role"></a>Pour installer le rôle de serveur Web (IIS)  
  
1.  Dans la console Gestionnaire de serveur, sur le **tableau de bord**, cliquez sur **ajouter des rôles et fonctionnalités**.  
  
2.  Cliquez sur **suivant** trois fois pour accéder à l’écran de sélection de rôle de serveur  
  
3.  Sur le **sélectionner des rôles de serveur** , sélectionnez **serveur Web (IIS)** , puis cliquez sur **suivant** quatre fois.  
  
4.  Dans la page **Confirmer les sélections d’installation** , cliquez sur **Installer**.  
  
5.  Vérifiez que l’installation a réussi, puis cliquez sur **fermer**.  
  
## <a name="bkmk_Share"></a>Créez un dossier partagé sur 2-APP1  
Créez un dossier partagé et un fichier texte dans le dossier sur 2-APP1.  
  
#### <a name="to-create-a-shared-folder"></a>Pour créer un dossier partagé  
  
1.  Sur le **Démarrer** , tapez**explorer.exe**, puis appuyez sur ENTRÉE.  
  
2.  Cliquez sur **ordinateur**, puis double-cliquez sur **disque Local (c)** .  
  
3.  Cliquez sur **nouveau dossier**, type **fichiers**, puis appuyez sur ENTRÉE. Laissez le **disque Local** fenêtre ouverte.  
  
4.  Sur le **Démarrer** , tapez**notepad.exe**, avec le bouton droit **le bloc-notes**, cliquez sur **avancé**, puis cliquez sur **d’identification administrateur**.  
  
5.  Dans le **sans titre - bloc-notes** fenêtre, tapez **il s’agit d’un fichier partagé sur 2-APP1**.  
  
6.  Cliquez sur **fichier**, cliquez sur **enregistrer**, cliquez sur **ordinateur**, double-cliquez sur **disque Local (c)** , puis double-cliquez sur le **fichiers**  dossier.  
  
7.  Dans **nom de fichier**, type **example.txt**, puis cliquez sur **enregistrer**. Fermez le Bloc-notes.  
  
8.  Dans le **disque Local** fenêtre, avec le bouton droit le **fichiers** dossier, pointez sur **partager avec**, puis cliquez sur **des personnes spécifiques**.  
  
9. Sur le **le partage de fichiers** boîte de dialogue, dans la liste déroulante, cliquez sur **tout le monde**, puis cliquez sur **ajouter**. Dans **niveau d’autorisation** pour **tout le monde**, cliquez sur **en lecture/écriture**.  
  
10. Cliquez sur **partage**, puis cliquez sur **fait**.  
  
11. Fermer le **disque Local** fenêtre.  
  



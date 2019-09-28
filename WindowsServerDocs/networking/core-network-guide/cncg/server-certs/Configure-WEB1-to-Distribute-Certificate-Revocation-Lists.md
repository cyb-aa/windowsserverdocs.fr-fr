---
title: Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5d53cbba37699346db110f0748a9c3e0c834c18e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356287"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le serveur Web WEB1 pour distribuer les listes de révocation de certificats.  
  
Dans les extensions de l’autorité de certification racine, il a été indiqué que la liste de révocation de certificats de l’autorité de certification racine serait disponible via https://pki.corp.contoso.com/pki. Actuellement, il n’existe pas de répertoire virtuel PKI sur WEB1. vous devez donc créer celui-ci.  
  
Pour effectuer cette procédure, vous devez être membre du **groupe Admins du domaine**.  
  
> [!NOTE]  
> Dans la procédure ci-dessous, remplacez le nom du compte d’utilisateur, le nom du serveur Web, les noms et les emplacements des dossiers, ainsi que d’autres valeurs par celles qui sont appropriées pour votre déploiement.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Pour configurer WEB1 pour distribuer des certificats et des listes de révocation de certificats  
  
1.  Sur WEB1, exécutez Windows PowerShell en tant qu’administrateur, tapez `explorer c:\`, puis appuyez sur entrée. L’Explorateur Windows s’ouvre sur le lecteur C.   
  
2.  Créez un nouveau dossier nommé PKI sur le lecteur C :. Pour ce faire, cliquez sur **démarrage**, puis sur **nouveau dossier**. Un nouveau dossier est créé avec le nom temporaire mis en surbrillance. Tapez **PKI** , puis appuyez sur entrée.  
  
3.  Dans l’Explorateur Windows, cliquez avec le bouton droit sur le dossier que vous venez de créer, placez le curseur de la souris sur **partager avec**, puis cliquez sur **personnes spécifiques**. La boîte de dialogue **Partage de fichiers** s'ouvre.  
  
4.  Dans **partage de fichiers**, tapez **éditeurs de certificats**, puis cliquez sur **Ajouter**. Le groupe éditeurs de certificats est ajouté à la liste. Dans la liste, dans **niveau d’autorisation**, cliquez sur la flèche en regard de **éditeurs de certificats**, puis cliquez sur **lecture/écriture**. Cliquez sur **partager**, puis sur **terminé**.  
  
5.  Fermez l’Explorateur Windows.  
  
6.  Ouvrez la console IIS. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire des services Internet (IIS)** .  
  
7.  Dans l’arborescence de la console du gestionnaire Internet Information Services (IIS), développez **web1**. Si vous êtes invité à effectuer une mise en route de la Plateforme Web Microsoft, cliquez sur **Annuler**.  
  
8.  Développez **Sites** , cliquez avec le bouton droit sur le **Site Web par défaut** , puis cliquez sur **Ajouter un répertoire virtuel**.  
  
9. Dans **alias**, tapez **PKI**. Dans **chemin d’accès physique** , tapez **C:\pki**, puis cliquez sur **OK**.  
  
10. Activez l’accès anonyme au répertoire virtuel PKI, afin qu’un client puisse vérifier la validité des certificats d’autorité de certification et des listes de révocation de certificats. Pour ce faire :  
  
    1.  Dans le volet **Connexions**, vérifiez que **pki** est sélectionné.  
  
    2.  Dans **Accueil pki** cliquez sur **Authentification**.  
  
    3.  Dans le volet **Actions** , cliquez sur **Modifier les autorisations**.  
  
    4.  Sous l’onglet **Sécurité** , cliquez sur **Modifier**.  
  
    5.  Dans la boîte de dialogue **Autorisations pour pki**, cliquez sur **Ajouter**.  
  
    6.  Dans **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes**, tapez **Anonymous Logon ; Tout le monde** , puis cliquez sur **vérifier les noms**. Cliquez sur **OK**.  
  
    7.  Cliquez sur **OK** dans la boîte de dialogue **Sélectionner les utilisateurs, les ordinateurs, les comptes de service ou les groupes** .  
  
    8.  Cliquez sur **OK** dans la boîte **de dialogue Autorisations pour PKI** .  
  
11. Cliquez sur **OK** dans la boîte de dialogue Propriétés de l' **infrastructure à clé publique** .  
  
12. Dans le volet **Accueil pki**, double-cliquez sur **Filtrage des demandes**.  
  
13. L’onglet **Extensions de noms de fichiers** est sélectionné par défaut dans le volet **Filtrage des demandes**. Dans le volet **Actions**, cliquez sur **Modifier les paramètres des fonctionnalités**.  
  
14. Dans **Modifier les paramètres de filtrage des demandes**, sélectionnez **Autoriser le double-échappement**, pus cliquez sur **OK**.  
  
15. Dans la console MMC du gestionnaire de Internet Information Services (IIS), cliquez sur le nom de votre serveur Web. Par exemple, si votre serveur Web est nommé WEB1, cliquez sur **web1**.  
  
16. Dans **actions**, cliquez sur **redémarrer**. Les services Internet sont arrêtés, puis redémarrés.  
  


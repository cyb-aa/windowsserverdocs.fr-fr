---
title: Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 57fa45eff87a1f0cdaae8b780d7f605e54ff6871
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839190"
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurer WEB1 pour distribuer des listes de révocation de certificats (CRL)

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le serveur web WEB1 pour distribuer des listes de révocation.  
  
Dans les extensions de l’autorité de certification racine, il a été indiqué que la liste CRL de l’autorité de certification racine serait disponible via https://pki.corp.contoso.com/pki. Actuellement, il n’est pas un répertoire virtuel PKI sur WEB1, donc vous devez en créer.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
> [!NOTE]  
> Dans la procédure ci-dessous, remplacez le nom de compte d’utilisateur, le nom du serveur Web, des noms de dossiers et des emplacements et des autres valeurs à celles qui sont appropriées pour votre déploiement.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Pour configurer WEB1 pour distribuer des certificats et listes CRL  
  
1.  Sur WEB1, exécutez Windows PowerShell en tant qu’administrateur, tapez `explorer c:\`, puis appuyez sur ENTRÉE. L’Explorateur Windows s’ouvre sur le lecteur C.   
  
2.  Créer un nouveau dossier nommé PKI sur le lecteur C:. Pour ce faire, cliquez sur **accueil**, puis cliquez sur **nouveau dossier**. Un nouveau dossier est créé avec le nom temporaire en surbrillance. Type **pki** puis appuyez sur ENTRÉE.  
  
3.  Dans l’Explorateur Windows, cliquez sur le dossier que vous venez de créer, pointez le curseur de la souris sur **partager avec**, puis cliquez sur **des personnes spécifiques**. La boîte de dialogue **Partage de fichiers** s'ouvre.  
  
4.  Dans **le partage de fichiers**, type **Cert Publishers**, puis cliquez sur **ajouter**. Le groupe éditeurs de certificats est ajouté à la liste. Dans la liste, dans **niveau d’autorisation**, cliquez sur la flèche en regard **Cert Publishers**, puis cliquez sur **en lecture/écriture**. Cliquez sur **partage**, puis cliquez sur **fait**.  
  
5.  Fermez l’Explorateur Windows.  
  
6.  Ouvrez la console IIS. Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Gestionnaire des services Internet (IIS)**.  
  
7.  Dans l’arborescence de la console Gestionnaire des Services Internet (IIS), développez **WEB1**. Si vous êtes invité à effectuer une mise en route de la Plateforme Web Microsoft, cliquez sur **Annuler**.  
  
8.  Développez **Sites** , cliquez avec le bouton droit sur le **Site Web par défaut** , puis cliquez sur **Ajouter un répertoire virtuel**.  
  
9. Dans **Alias**, type **pki**. Dans **chemin d’accès physique** type **C:\pki**, puis cliquez sur **OK**.  
  
10. Activer anonyme accéder au répertoire virtuel pki, afin que n’importe quel client puisse vérifier la validité des certificats d’autorité de certification et des CRL. Pour ce faire :  
  
    1.  Dans le volet **Connexions**, vérifiez que **pki** est sélectionné.  
  
    2.  Dans **Accueil pki** cliquez sur **Authentification**.  
  
    3.  Dans le volet **Actions** , cliquez sur **Modifier les autorisations**.  
  
    4.  Sous l’onglet **Sécurité** , cliquez sur **Modifier**.  
  
    5.  Dans la boîte de dialogue **Autorisations pour pki**, cliquez sur **Ajouter**.  
  
    6.  Dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes**, type **ANONYMOUS LOGON ; Tout le monde** puis cliquez sur **vérifier les noms**. Cliquez sur **OK**.  
  
    7.  Cliquez sur **OK** sur le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes** boîte de dialogue.  
  
    8.  Cliquez sur **OK** sur le **autorisations pour pki** boîte de dialogue.  
  
11. Cliquez sur **OK** sur le **pki propriétés** boîte de dialogue.  
  
12. Dans le volet **Accueil pki**, double-cliquez sur **Filtrage des demandes**.  
  
13. L’onglet **Extensions de noms de fichiers** est sélectionné par défaut dans le volet **Filtrage des demandes**. Dans le volet **Actions**, cliquez sur **Modifier les paramètres des fonctionnalités**.  
  
14. Dans **Modifier les paramètres de filtrage des demandes**, sélectionnez **Autoriser le double-échappement**, pus cliquez sur **OK**.  
  
15. Dans la console MMC Gestionnaire Internet Information Services (IIS), cliquez sur le nom de votre serveur Web. Par exemple, si votre serveur Web est nommé WEB1, cliquez sur **WEB1**.  
  
16. Dans **Actions**, cliquez sur **redémarrer**. Les services Internet sont arrêtés, puis redémarrés.  
  


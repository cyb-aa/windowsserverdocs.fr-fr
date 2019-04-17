---
title: Configurer WEB1 pour distribuer les listes de révocation de certificats (CRL)
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: fa4a8c41-8c2a-425c-8511-736fe5d196ac
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b96fad769638de9835c52137e5165fa32a6b9bd4
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-web1-to-distribute-certificate-revocation-lists-crls"></a>Configurer WEB1 pour distribuer les listes de révocation de certificats (CRL)

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer le serveur web WEB1 pour distribuer les listes de révocation.  
  
Dans les extensions de l’autorité de certification racine, il a été indiqué que la liste CRL de l’autorité de certification racine serait disponible via http://pki.corp.contoso.com/pki. Actuellement, il n’est pas un répertoire virtuel PKI sur WEB1, afin d’un doit être créé.  
  
Pour effectuer cette procédure, vous devez être membre du **Admins du domaine**.  
  
> [!NOTE]  
> Dans la procédure ci-dessous, remplacez le nom de compte d’utilisateur, le nom du serveur Web, les noms de dossier et emplacements et d’autres valeurs par celles qui sont appropriées pour votre déploiement.  
  
#### <a name="to-configure-web1-to-distribute-certificates-and-crls"></a>Pour configurer WEB1 pour distribuer des certificats et listes CRL  
  
1.  Sur WEB1, exécutez Windows PowerShell en tant qu’administrateur, tapez `explorer c:\`, puis appuyez sur ENTRÉE. L’Explorateur Windows s’ouvre sur le lecteur C.   
  
2.  Créer un nouveau dossier nommé PKI sur le lecteur C:. Pour ce faire, cliquez sur **accueil**, puis cliquez sur **nouveau dossier**. Un nouveau dossier est créé avec le nom temporaire mis en surbrillance. Type **pki** et appuyez sur ENTRÉE.  
  
3.  Dans l’Explorateur Windows, cliquez sur le dossier que vous venez de créer, placez le curseur de la souris dans **partager avec**, puis cliquez sur **des personnes spécifiques**. Le **le partage de fichiers** boîte de dialogue s’ouvre.  
  
4.  Dans **le partage de fichiers**, type **Cert Publishers**, puis cliquez sur **ajouter**. Le groupe éditeurs de certificats est ajouté à la liste. Dans la liste, dans **niveau d’autorisation**, cliquez sur la flèche en regard **Cert Publishers**, puis cliquez sur **en lecture/écriture**. Cliquez sur **partage**, puis cliquez sur **fait**.  
  
5.  Fermez l’Explorateur Windows.  
  
6.  Ouvrez la console IIS. Dans le Gestionnaire de serveur, cliquez sur **outils**, puis cliquez sur **Gestionnaire des Services Internet (IIS)**.  
  
7.  Dans l’arborescence de la console Gestionnaire des Services Internet (IIS), développez **WEB1**. Si vous êtes invité à démarrer avec la plateforme Web Microsoft, cliquez sur **Annuler**.  
  
8.  Développez **Sites**, puis cliquez sur le **site Web par défaut** puis cliquez sur **ajouter un répertoire virtuel**.  
  
9. Dans **Alias**, type **pki**. Dans **chemin d’accès physique** type **C:\pki**, puis cliquez sur **OK**.  
  
10. Activer anonyme accéder au répertoire virtuel pki, afin que n’importe quel client peut vérifier la validité des certificats d’autorité de certification et des listes de révocation. Pour ce faire:  
  
    1.  Dans le **connexions** volet, assurez-vous que **pki** est sélectionné.  
  
    2.  Sur **accueil pki** cliquez sur **authentification**.  
  
    3.  Dans le **Actions** volet, cliquez sur **modifier les autorisations**.  
  
    4.  Sur le **sécurité**, cliquez sur **modifier**  
  
    5.  Sur le **autorisations pour pki** boîte de dialogue, cliquez sur **ajouter**.  
  
    6.  Dans le **sélectionner des utilisateurs, les ordinateurs, les comptes de Service ou les groupes**, type **ouverture de session anonyme; tout le monde** puis cliquez sur **vérifier les noms**. Cliquez sur **OK**.  
  
    7.  Cliquez sur **OK** sur le **sélectionner des utilisateurs, ordinateurs ou les groupes, comptes de Service** boîte de dialogue.  
  
    8.  Cliquez sur **OK** sur le **autorisations pour pki** boîte de dialogue.  
  
11. Cliquez sur **OK** sur le **pki propriétés** boîte de dialogue.  
  
12. Dans le **accueil pki** volet, double-cliquez sur **filtrage des demandes**.  
  
13. Le **Extensions de nom de fichier** onglet est sélectionné par défaut dans le **filtrage des demandes** volet. Dans le **Actions** volet, cliquez sur **modifier les paramètres de fonctionnalité**.  
  
14. Dans **modifier les paramètres de filtrage des demandes**, sélectionnez **autoriser le double échappement** puis cliquez sur **OK**.  
  
15. Dans la console MMC Gestionnaire Internet Information Services (IIS), cliquez sur le nom de votre serveur Web. Par exemple, si votre serveur Web est nommé WEB1, cliquez sur **WEB1**.  
  
16. Dans **Actions**, cliquez sur **redémarrer**. Les services Internet sont arrêtés, puis redémarrés.  
  


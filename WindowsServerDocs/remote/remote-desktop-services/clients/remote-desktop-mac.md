---
title: Bien démarrer avec le Bureau à distance sur Mac
description: Découvrez comment configurer le client Bureau à distance pour Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7afc65f8-3158-49c9-9d48-4dab1c69afba
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 10/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: e8c5da1960d0e3129b5520e65c2d5ecf45eef778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886950"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Bien démarrer avec le Bureau à distance sur Mac

>S'applique à : Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Vous pouvez utiliser le client Bureau à distance pour Mac pour travailler avec les applications Windows, les ressources et les postes de travail à partir de votre ordinateur Mac. Utilisez les informations suivantes pour commencer et extraire le [FAQ](remote-desktop-client-faq.md) si vous avez des questions.

>[!Note]
> - Curieux de savoir les nouvelles versions du client Mac OS ? Découvrez [quelles sont les nouveautés pour Bureau à distance sur Mac ?](mac-whatsnew.md)
> - Le client Mac s’exécute sur les ordinateurs exécutant Mac OS 10.10 et versions ultérieures.
> - Les informations contenues dans cet article s’applique principalement à la version complète du client Mac - la version disponible dans l’App Store Mac. Testez les nouvelles fonctionnalités en téléchargeant notre application de la version préliminaire ici : [notes de mise à jour du client bêta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obtenir le client Bureau à distance
Suivez ces étapes pour bien démarrer avec le Bureau à distance sur votre Mac :

1. Télécharger le client Bureau à distance Microsoft à partir de la [Mac App Store](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurer votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Si vous ignorez cette étape, vous ne pouvez pas vous connecter à votre PC.)
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour vous connecter directement à un PC Windows et à une ressource distante à utiliser un programme RemoteApp, un bureau basé sur session ou un bureau virtuel publié en local à l’aide de connexions RemoteApp et bureau. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

## <a name="what-about-the-mac-beta-client"></a>Qu’en est-il du client de la version bêta Mac ?
Nous allons tester les nouvelles fonctionnalités sur notre canal en version préliminaire sur HockeyApp. Vous voulez extraire ? Accédez à [Bureau à distance Microsoft pour Mac](https://go.microsoft.com/fwlink/?LinkID=619698) et cliquez sur **télécharger**. Vous n’avez pas besoin de créer un compte ou la connexion à HockeyApp pour télécharger le client de la version bêta.

Si vous avez déjà le client, vous pouvez vérifier les mises à jour pour vous assurer la dernière version. Dans le client de la version bêta, cliquez sur **bêta de bureau à distance Microsoft** en haut et puis cliquez sur **vérifier les mises à jour**. 

## <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance
Pour créer une connexion Bureau à distance :

1. Dans le centre de connexion, cliquez sur **+**, puis cliquez sur **Desktop**.
2. Entrez les informations suivantes :
   - **Nom du PC** -le nom de l’ordinateur.
      - Cela peut être un nom d’ordinateur Windows (trouvée dans le **système** paramètres), un nom de domaine ou une adresse IP.
      - Vous pouvez également ajouter les informations de port à la fin de ce nom, comme *MyDesktop:3389*.
   - **Compte d’utilisateur** -ajouter le compte d’utilisateur vous permet d’accéder à l’ordinateur distant.
      - Pour Active Directory (AD) joints à des ordinateurs ou des comptes locaux, utilisez une de ces formats : *user_name*, *domaine\nom_utilisateur*, ou *user_name@domain.com*.
      - Pour Azure Active Directory (AAD) joint à un ordinateurs, utilisez une de ces formats : *AzureAD\user_name* ou *AzureAD\user_name@domain.com*.
      - Vous pouvez également choisir s’il faut exiger un mot de passe.
      - Lors de la gestion de plusieurs comptes d’utilisateur portant le même nom d’utilisateur, définissez un nom convivial pour différencier les comptes.
      - Gérer vos comptes d’utilisateur enregistré dans les préférences de l’application. 

3. Vous pouvez également définir ces paramètres facultatifs pour la connexion :
   - Définissez un nom convivial 
   - Ajouter une passerelle
   - Définissez la sortie audio
   - Permuter les boutons de la souris
   - Activer le Mode administrateur
   - Redirection de dossiers locaux dans une session à distance
   - Imprimantes locales vers l’avant
   - Cartes à puce vers l’avant
4. Cliquez sur **Enregistrer**.

Pour démarrer la connexion, double-cliquez simplement sur elle. Cela vaut également pour les ressources distantes.

### <a name="export-and-import-connections"></a>Exporter et importer des connexions
Vous pouvez exporter une définition de la connexion Bureau à distance et l’utiliser sur un autre appareil. Bureaux à distance est enregistrés dans distinct. Fichiers RDP.

1. Dans le centre de connexion, cliquez sur le Bureau à distance.
2. Cliquez sur **Exporter**.
3. Accédez à l’emplacement où vous souhaitez enregistrer le Bureau à distance. Fichier RDP.
4. Cliquez sur **OK**.

Utilisez les étapes suivantes pour importer un bureau à distance. Fichier RDP.

1. Dans la barre de menus, cliquez sur **fichier > importation**.
2. Accédez à la. Fichier RDP.
3. Cliquez sur **Ouvrir**.

## <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Ressources distantes sont des programmes RemoteApp, bureaux basés sur session et des bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

- L’URL affiche le lien au serveur d’accès Web de bureau à distance qui vous donne accès aux connexions RemoteApp et bureau.
- Le configuré connexions RemoteApp et bureau sont répertoriées.

Pour ajouter une ressource distante :

1. Dans, cliquez sur le centre de connexion **+**, puis cliquez sur **ajouter des ressources à distance**. 
2. Entrez les informations pour la ressource distante :
   - **URL du flux** -l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ : cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associé à votre adresse de messagerie.
   - **Nom d’utilisateur** -nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
   - **Mot de passe** -mot de passe à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
3. Cliquez sur **Enregistrer**.


Les ressources à distance seront affichera dans le centre de connexion.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle des services Bureau à distance (passerelle RD) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles dans les préférences de l’application ou lors de la définition d’une nouvelle connexion de bureau.

Pour configurer une passerelle dans les préférences :

1. Dans le centre de connexion, cliquez sur **Préférences > passerelles**. 
2. Cliquez sur le **+** bouton en bas de la table, entrez les informations suivantes :
  - **Nom du serveur** – le nom de l’ordinateur que vous souhaitez utiliser en tant que passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
  - **Nom d’utilisateur** -le nom d’utilisateur et le mot de passe à utiliser pour la passerelle Bureau à distance que vous êtes connecté. Vous pouvez également sélectionner **utiliser les informations d’identification de connexion** à utiliser le même nom d’utilisateur et le mot de passe que ceux utilisés pour la connexion Bureau à distance.


## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Lorsque vous vous connectez à un bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur à sélectionner à nouveau. Vous pouvez gérer vos comptes d’utilisateur en utilisant le client Bureau à distance.

Pour créer un nouveau compte d’utilisateur :

1. Dans le centre de connexion, cliquez sur **paramètres** > **comptes**.
2. Cliquez sur **ajouter le compte d’utilisateur**.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** -le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants : user_name, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** -le mot de passe pour l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour utiliser des connexions à distance doit avoir un mot de passe associé.
   - **Nom convivial** : Si vous utilisez le même compte d’utilisateur avec des mots de passe différents, définissez un nom convivial pour distinguer ces comptes d’utilisateur.
4. Appuyez sur **enregistrer**, puis appuyez sur **paramètres**.

## <a name="customize-your-display-resolution"></a>Personnaliser votre résolution d’affichage
Vous pouvez spécifier la résolution d’affichage pour la session Bureau à distance.

1. Dans le centre de connexion, cliquez sur **préférences**.
2. Cliquez sur **résolution**. 
3. Cliquez sur **+**.
4. Entrez une hauteur de résolution et une largeur, puis cliquez sur **OK.**

Pour supprimer la résolution, sélectionnez-le, puis cliquez sur **-**.

**Affiche des espaces distincts** si vous exécutez Mac OS X 10.9 et désactivé **affiche des espaces distincts** dans Mavericks (**Préférences système > Centre de contrôle**), vous devez configurer Ce paramètre dans le client Bureau à distance à l’aide de la même option.

### <a name="drive-redirection-for-remote-resources"></a>Redirection de lecteur pour les ressources distantes
La redirection de lecteur est pris en charge pour les ressources distantes, afin que vous pouvez enregistrer les fichiers créés avec une application distante localement sur votre Mac. Le dossier redirigé est toujours affichée comme un lecteur réseau dans la session à distance à votre répertoire de base.

> [!NOTE]
> Pour utiliser cette fonctionnalité, l’administrateur doit définir les paramètres appropriés sur le serveur.


## <a name="use-a-keyboard-in-a-remote-session"></a>Utiliser un clavier dans une session à distance

Les dispositions de clavier Windows diffèrent des dispositions du clavier Mac. 

- La clé de commande sur le clavier Mac est égale à la clé de Windows.
- Pour effectuer des actions qui utilisent le bouton de commande sur le Mac, vous devez utiliser le bouton de contrôle dans Windows (par exemple : Copy = Ctrl + C).
- Les touches de fonction peuvent être activés dans la session en outre la touche FN (par exemple : FN + F1).
- La touche Alt à droite de la barre d’espace sur le clavier Mac est égale à la touche Alt Gr/droite Alt dans Windows.

Par défaut, la session à distance utilisera les mêmes paramètres régionaux de clavier en tant que le système d’exploitation que vous exécutez le client sur. (Si votre Mac est en cours d’exécution en-us du système d’exploitation, qui sera utilisé pour les sessions à distance ainsi. Si les paramètres régionaux clavier de système d’exploitation ne sont pas utilisé, vérifiez que le clavier définissant sur un ordinateur distant et en modifiant le paramètre manuellement. Consultez le [Forum aux questions du Client Bureau à distance](remote-desktop-client-faq.md) pour plus d’informations sur les claviers et paramètres régionaux.


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Prise en charge d’authentification enfichables de passerelle Bureau à distance et d’autorisation

Windows Server 2012 R2 a introduit la prise en charge pour une nouvelle méthode d’authentification, d’authentification enfichables de passerelle Bureau à distance et d’autorisation, qui offre plus de souplesse pour les routines d’authentification personnalisée. Vous pouvez maintenant ce modèle d’authentification avec le client Mac. 

> [!IMPORTANT]
> Authentification et autorisation des modèles personnalisés avant Windows 8.1 n'est pas pris en charge, bien que l’article ci-dessus aborde les.

Pour en savoir plus sur cette fonctionnalité, consultez [ http://aka.ms/paa-sample ](http://aka.ms/paa-sample).


> [!TIP]
> Questions et vos commentaires sont toujours les bienvenus. Toutefois, ne posez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez à la [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité ? Dites-nous dans le [forum vocal des utilisateurs client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).


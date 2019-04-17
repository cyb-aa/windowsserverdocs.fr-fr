---
title: Vous familiariser avec les services Bureau à distance sur Mac
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
ms.sourcegitcommit: 6dc14d4315793132488494b5543ee83e3f562f09
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4555716"
---
# Vous familiariser avec les services Bureau à distance sur Mac

>S’applique à: Windows10, Windows8.1, Windows Server2012R2, WindowsServer2016

Vous pouvez utiliser le client Bureau à distance pour Mac pour fonctionner avec les applications Windows, les ressources et les postes de travail à partir de votre ordinateur Mac. Utilisez les informations suivantes pour commencer - et consultez le [Forum aux questions sur](remote-desktop-client-faq.md) si vous avez des questions.

>[!Note]
> - Curieux de savoir sur les nouvelles versions du client macOS? Consultez l’article [Nouveautés pour les services Bureau à distance sur Mac?](mac-whatsnew.md)
> - Le client Mac s’exécute sur les ordinateurs exécutant Mac OS 10.10 et versions ultérieures.
> - Les informations contenues dans cet article s’applique principalement à la version complète du client Mac - la version disponible dans l’AppStore Mac. Testez les nouvelles fonctionnalités en téléchargeant notre application aperçu ici: [notes de publication du client de la version bêta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## Obtenir le client Bureau à distance
Suivez ces étapes pour vous familiariser avec les services Bureau à distance sur votre Mac:

1. Téléchargez le client Bureau à distance Microsoft à partir du [Magasin de l’application Mac](https://itunes.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configuration de votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). (Si vous ignorez cette étape, vous ne pouvez pas vous connecter à votre PC.)
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour se connecter directement à un PC Windows et une ressource distante à utiliser un programme RemoteApp, d’un ordinateur de bureau basée sur la session, ou un ordinateur de bureau virtuel publiés sur site à l’aide de connexions RemoteApp et bureau. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

## Qu’en est-il le client de la version bêta Mac?
Nous allons tester les nouvelles fonctionnalités sur notre chaîne de version d’évaluation sur HockeyApp. Vous voulez essayez-la? Accédez à [Microsoft de bureau à distance pour Mac](https://go.microsoft.com/fwlink/?LinkID=619698) et cliquez sur **Télécharger**. Vous n’avez pas besoin de créer un compte ou une connexion en HockeyApp à télécharger le client de la version bêta.

Si vous avez déjà le client, vous pouvez vérifier les mises à jour vous assurer que la dernière version. Dans le client de la version bêta, cliquez sur **La version de bêta de bureau à distance Microsoft** en haut, puis cliquez sur **Rechercher les mises à jour**. 

## Ajouter une connexion Bureau à distance
Pour créer une connexion Bureau à distance:

1. Dans le centre de la connexion, cliquez sur **+**, puis cliquez sur le **Bureau**.
2. Entrez les informations suivantes:
   - **Nom du PC** - le nom de l’ordinateur.
      - Cela peut être un nom d’ordinateur Windows (disponible dans les paramètres **système** ), un nom de domaine ou une adresse IP.
      - Vous pouvez également ajouter des informations de port à la fin de ce nom, comme *MyDesktop:3389*.
   - **Compte d’utilisateur** : ajouter le compte d’utilisateur que vous utilisez pour accéder à l’ordinateur distant.
      - Pour Active Directory (AD) joint des ordinateurs ou les comptes locaux, utilisez l’une de ces formats: *nom_utilisateur*, *domaine\nom_utilisateur*ou *user_name@domain.com*.
      - Pour Azure Active Directory (AAD) joint des ordinateurs, utilisez l’une de ces formats: *AzureAD\user_name* ou *AzureAD\ user_name@domain.com*.
      - Vous pouvez également choisir s’il convient d’exiger un mot de passe.
      - La gestion de plusieurs comptes d’utilisateur avec le même nom d’utilisateur, attribuer un nom convivial pour différencier les comptes.
      - Gérer vos comptes d’utilisateur enregistré dans les préférences de l’application. 

3. Vous pouvez également définir ces paramètres facultatifs pour la connexion:
   - Définir un nom convivial 
   - Ajouter une passerelle
   - Définissez la sortie audio
   - Échange des boutons de la souris
   - Activer le Mode administrateur
   - Redirection de dossiers locaux dans une session à distance
   - Imprimantes locales vers l’avant
   - Cartes à puce vers l’avant
4. Cliquez sur **Enregistrer**.

Pour démarrer la connexion, double-cliquez sur elle. Il en est de même pour les ressources distantes.

### Exporter et importer des connexions
Vous pouvez exporter une définition de la connexion Bureau à distance et l’utiliser sur un autre appareil. Ordinateurs de bureau à distance sont enregistrés dans distinct. Fichiers RDP.

1. Dans le centre de la connexion, cliquez sur le Bureau à distance.
2. Cliquez sur **Exporter**.
3. Accédez à l’emplacement où vous souhaitez enregistrer le Bureau à distance. Fichier RDP.
4. Cliquez sur **OK**.

Utilisez les étapes suivantes pour importer un ordinateur de bureau à distance. Fichier RDP.

1. Dans la barre de menus, cliquez sur **fichier > Importation**.
2. Accédez à la. Fichier RDP.
3. Cliquez sur **Ouvrir**.

## Ajouter une ressource à distance
Ressources distantes sont des programmes RemoteApp, postes de travail basée sur la session et les bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

- L’URL affiche le lien vers le serveur d’accès Web de bureau à distance qui vous donne accès aux connexions RemoteApp et bureau.
- Programmes RemoteApp et les connexions Bureau configuré sont répertoriées.

Pour ajouter une ressource à distance:

1. Dans, cliquez sur le centre de connexion **+**, puis cliquez sur **Ajouter les ressources distantes**. 
2. Entrez les informations de la ressource à distance:
   - **URL du flux** - l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ, cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associées à votre adresse de messagerie.
   - **Nom d’utilisateur** : le nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
   - **Mot de passe** - le mot de passe à utiliser pour le serveur d’accès Web de bureau à distance à que vous vous connectez.
3. Cliquez sur **Enregistrer**.


Les ressources distantes seront affichera dans le centre de la connexion.


## Se connecter à une passerelle des services Bureau à distance pour accéder à des ressources internes

Une passerelle des services Bureau à distance (passerelle des services Bureau à distance) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles dans les préférences de l’application ou lors de la définition d’une nouvelle connexion Bureau.

Pour configurer une nouvelle passerelle dans les préférences:

1. Dans le centre de la connexion, cliquez sur **Préférences > passerelles**. 
2. Cliquez sur le **+** bouton en bas de la table, entrez les informations suivantes:
  - **Nom du serveur** : le nom de l’ordinateur que vous souhaitez utiliser sous forme de passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du serveur (par exemple: **RDGateway:443** ou **10.0.0.1:443**).
  - **Nom d’utilisateur** : le nom d’utilisateur et mot de passe à utiliser pour la passerelle des services Bureau à distance à que vous vous connectez. Vous pouvez également sélectionner **utiliser les informations d’identification de connexion** pour utiliser le même nom d’utilisateur et mot de passe que ceux utilisés pour la connexion Bureau à distance.


## Gérer vos comptes d’utilisateurs

Lorsque vous vous connectez à un ordinateur de bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur pour sélectionner à partir d’une nouvelle fois. Vous pouvez gérer vos comptes d’utilisateur à l’aide du client Bureau à distance.

Pour créer un compte d’utilisateur:

1. Dans le centre de la connexion, cliquez sur **les paramètres** > **comptes**.
2. Cliquez sur **Ajouter un compte d’utilisateur**.
3. Entrez les informations suivantes:
   - **Nom d’utilisateur** : le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants: nom_utilisateur, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** - le mot de passe pour l’utilisateur que vous avez spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer de manière à utiliser pour les connexions à distance doit avoir un mot de passe associé.
   - **Nom convivial** : Si vous utilisez le même compte d’utilisateur avec des mots de passe différents, définissez un nom convivial pour distinguer ces comptes d’utilisateur.
4. Appuyez sur **Enregistrer**, puis appuyez sur **paramètres**.

## Personnaliser la résolution d’affichage
Vous pouvez spécifier la résolution d’affichage de la session Bureau à distance.

1. Dans le centre de la connexion, cliquez sur **Préférences**.
2. Cliquez sur **la résolution**. 
3. Cliquez sur **+**.
4. Entrez une hauteur de résolution et une largeur, puis cliquez sur **OK.**

Pour supprimer la résolution, sélectionnez-le, puis cliquez sur **-**.

**Affiche des espaces distincts** Si vous exécutez Mac OS X 10.9 et désactivé **affiche des espaces distincts** dans Mavericks (**Préférences système > contrôle des missions**), vous devez configurer ce paramètre dans le client Bureau à distance à l’aide de la même option.

### Redirection de lecteur pour les ressources distantes
La redirection de lecteur est pris en charge pour les ressources distantes, afin que vous pouvez enregistrer des fichiers créés avec une application à distance en local sur votre Mac. Le dossier redirigé est toujours affiché comme un lecteur réseau dans la session à distance à votre répertoire de base.

> [!NOTE]
> Pour pouvoir utiliser cette fonctionnalité, l’administrateur doit définir les paramètres appropriés sur le serveur.


## Utiliser un clavier dans une session à distance

Les dispositions de clavier Windows diffèrent des dispositions de clavier de Mac. 

- La touche de commande sur le clavier Mac est égale à la touche Windows.
- Pour effectuer des actions qui utilisent le bouton de commande sur le Mac, vous devez utiliser le bouton de contrôle de Windows (par exemple: copie = Ctrl + C).
- Les touches de fonction peuvent être activées dans la session en appuyant sur en outre la touche FN (par exemple: FN + F1).
- La touche Alt à droite de la barre d’espace sur le clavier Mac est égale à la touche Alt Gr/droite Alt dans Windows.

Par défaut, la session à distance utilisera les mêmes paramètres régionaux de clavier en tant que le système d’exploitation que vous exécutez le client. (Si votre Mac est en cours d’exécution en-us du système d’exploitation, qui est utilisés pour les sessions à distance ainsi. Si les paramètres régionaux clavier de système d’exploitation ne sont pas utilisé, vérifiez que le clavier définissant sur le PC distant et en modifiant le paramètre manuellement. Consultez le [Forum aux questions du Client Bureau à distance](remote-desktop-client-faq.md) pour plus d’informations sur les claviers et les paramètres régionaux.


## Prise en charge pour l’authentification de passerelle enfichable Bureau à distance et d’autorisation

Windows Server 2012 R2 a introduit la prise en charge pour une nouvelle méthode d’authentification, l’authentification enfichable de passerelle des services Bureau à distance et d’autorisation, qui fournit une plus grande souplesse pour les routines d’authentification personnalisée. Vous pouvez maintenant ce modèle d’authentification avec le client Mac. 

> [!IMPORTANT]
> Modèles d’authentification et d’autorisation personnalisés avant Windows 8.1 ne sont pas pris en charge, bien que l’article ci-dessus présente les.

Pour en savoir plus sur cette fonctionnalité, consultez l’article [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Questions et commentaires sont toujours accueil. Toutefois, veuillez ne publiez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez au [Forum sur le client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité? Faites-nous part de sur le [forum de voix utilisateur client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).


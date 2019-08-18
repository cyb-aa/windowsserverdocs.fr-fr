---
title: Prise en main du Bureau à distance sur Mac
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
ms.openlocfilehash: 17df3ca3b88404a2775790d7a4a8206b7aa5befa
ms.sourcegitcommit: 0467b8e69de66e3184a42440dd55cccca584ba95
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69546342"
---
# <a name="get-started-with-remote-desktop-on-mac"></a>Prise en main du Bureau à distance sur Mac

>S'applique à : Windows 10, Windows 8.1, Windows Server 2012 R2, Windows Server 2016

Le client Bureau à distance sur Mac vous permet d’accéder à des applications, ressources et bureaux Windows à partir de votre ordinateur Mac. Consultez les informations suivantes. Elles constituent un excellent point de départ. Et si vous avez des questions par la suite, n’hésitez pas à consulter notre [FAQ](remote-desktop-client-faq.md).

>[!NOTE]
> - Les nouvelles versions du client macOS vous intéressent ? Consultez [Nouveautés du Bureau à distance sur Mac](mac-whatsnew.md).
> - Le client Mac s’exécute sur les ordinateurs exécutant Mac OS 10.10 et versions ultérieures.
> - Les informations contenues dans cet article s’appliquent principalement à la version complète du client Mac (celle disponible dans l’App Store Mac). Testez les nouvelles fonctionnalités en téléchargeant la préversion de notre application ici : [Notes de mise à jour du client de la version bêta](https://go.microsoft.com/fwlink/?LinkID=619698&clcid=0x409).

## <a name="get-the-remote-desktop-client"></a>Obtenez le client Bureau à distance
Effectuez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil Mac:

1. Téléchargez le client Bureau à distance Microsoft à partir de l’[App Store Mac](https://itunes.apple.com/app/microsoft-remote-desktop/id1295203466?mt=12).
2. [Configurez votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop). Si vous ignorez cette étape, vous ne pouvez pas vous connecter à votre PC.
3. Ajoutez une connexion Bureau à distance ou une ressource distante. Utilisez une connexion pour vous connecter directement à un PC Windows, et une ressource distante pour accéder à un programme RemoteApp, un bureau basé sur une session ou un bureau virtuel publié en local à l’aide de la fonctionnalité Connexions aux programmes RemoteApp et aux services Bureau à distance. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

## <a name="what-about-the-mac-beta-client"></a>Présentation du client de la version bêta Mac
Nous testons de nouvelles fonctionnalités sur notre canal de préversion sur HockeyApp. Vous voulez en savoir plus ? Rendez-vous sur la page [Bureau à distance Microsoft pour Mac](https://go.microsoft.com/fwlink/?LinkID=619698) et cliquez sur **Télécharger**. Vous n’avez pas besoin de créer un compte ou de vous connecter à HockeyApp pour télécharger le client de la version bêta.

Si vous avez déjà le client, vous pouvez vérifier si des mises à jour sont disponibles, pour être certain d’avoir la version la plus récente. En haut de la fenêtre du client de la version bêta, cliquez sur **Version bêta de Bureau à distance Microsoft**, puis sur **Rechercher des mises à jour**. 

## <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance
Pour créer une connexion Bureau à distance :

1. Dans le Centre de connexion, cliquez sur **+** , puis sur **Bureau**.
2. Entrez les informations suivantes :
   - **Nom du PC** : nom de l’ordinateur.
      - Il peut s’agir d’un nom d’ordinateur Windows (trouvé dans les paramètres **Système**), d’un nom de domaine ou d’une adresse IP.
      - Vous pouvez également ajouter des informations de port à la fin de ce nom, comme *MonBureau:3389*.
   - **Compte d’utilisateur** : compte d’utilisateur à utiliser pour accéder au PC distant.
     - Pour les ordinateurs ou les comptes locaux reliés à Active Directory (AD), utilisez l’un de ces formats : *nom_utilisateur*, *domaine\nom_utilisateur*, ou <em>user_name@domain.com</em>.
     - Pour les ordinateurs reliés à Azure Active Directory (AAD), utilisez l’un de ces formats : *AzureAD\nom_utilisateur* ou <em>AzureAD\user_name@domain.com</em>.
     - Vous pouvez également choisir d’exiger un mot de passe.
     - Lorsque vous gérez plusieurs comptes utilisateurs avec le même nom d’utilisateur, définissez un nom convivial pour différencier les comptes.
     - Gérez vos comptes d’utilisateurs enregistrés dans les préférences de l’application. 

3. Vous pouvez également définir ces paramètres optionnels pour la connexion :
   - Définir un nom convivial 
   - Ajouter une passerelle
   - Définir la sortie audio
   - Permuter les boutons de la souris
   - Activer le mode Administrateur
   - Rediriger les dossiers locaux vers une session à distance
   - Transférer les imprimantes locales
   - Transférer les cartes à puce
4. Cliquez sur **Enregistrer**.

Pour démarrer la connexion, il suffit de double-cliquer dessus. Il en va de même pour les ressources distantes.

### <a name="export-and-import-connections"></a>Exporter et importer des connexions
Vous pouvez exporter une définition de la connexion Bureau à distance et l’utiliser sur un autre appareil. Les Bureaux à distance sont enregistrés dans des fichiers .RDP distincts.

1. Dans le Centre de connexion, faites un clic droit sur le Bureau à distance.
2. Cliquez sur **Exporter**.
3. Accédez à l’emplacement où vous souhaitez enregistrer le fichier .RDP du Bureau à distance.
4. Cliquez sur **OK**.

Suivez cette procédure pour importer un fichier .RDP de Bureau à distance :

1. Dans la barre de menus, cliquez sur **Fichier** > **Importer**.
2. Accédez au fichier .RDP.
3. Cliquez sur **Ouvrir**.

## <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés à l’aide de la fonctionnalité Connexions aux programmes RemoteApp et aux services Bureau à distance.

- L’URL affiche le lien vers le serveur d’accès Web des services Bureau à distance qui vous donne accès aux connexions RemoteApp et Bureau à distance.
- Les connexions RemoteApp et Bureau à distance configurées sont affichées.

Pour ajouter une ressource distante :

1. Dans le Centre de connexion, cliquez sur **+** , puis sur **Ajouter des ressources distantes**. 
2. Entrez les informations appropriées pour la ressource distante :
   - **URL de flux** : URL du serveur d’accès web des services Bureau à distance. Vous pouvez également entrer votre compte e-mail professionnel dans ce champ : cela indique au client de rechercher le serveur d’accès Web des services Bureau à distance qui est associé à votre adresse e-mail.
   - **Nom d’utilisateur** : nom d’utilisateur à spécifier pour le serveur d’accès web des services Bureau à distance auquel vous vous connectez.
   - **Mot de passe** : mot de passe à spécifier pour le serveur d’accès web des services Bureau à distance auquel vous vous connectez.
3. Cliquez sur **Enregistrer**.


Les ressources distantes ajoutées seront affichées dans le Centre de connexion.


## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle Bureau à distance vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles dans les préférences de l’application ou lors de l’établissement d’une nouvelle connexion Bureau.

Pour configurer une nouvelle passerelle dans les préférences :

1. Dans le Centre de connexion, cliquez sur **Préférences > Passerelles**. 
2. Cliquez sur le bouton **+** situé en bas de la table, puis entrez les informations suivantes :
   - **Nom du serveur** : nom de l’ordinateur que vous souhaitez utiliser comme passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
   - **Nom d’utilisateur** : nom d’utilisateur et mot de passe à spécifier pour la passerelle Bureau à distance à laquelle vous vous connectez. Vous pouvez également sélectionner **Utiliser les informations d’identification de la connexion** si vous préférez garder les mêmes nom d’utilisateur et mot de passe que ceux utilisés pour la connexion Bureau à distance.


## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Quand vous vous connectez à un bureau ou à des ressources distantes, vous pouvez enregistrer les comptes d’utilisateur pour les resélectionner ultérieurement. Vous pouvez gérer vos comptes d’utilisateur à l’aide du client Bureau à distance.

Pour créer un compte d’utilisateur :

1. Dans le Centre de connexion, cliquez sur **Paramètres** > **Comptes**.
2. Cliquez sur **Ajouter un compte d’utilisateur**.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** : nom d’utilisateur à enregistrer pour l’utiliser avec une connexion à distance. Entrez le nom d’utilisateur dans un de ces formats : nom_utilisateur, domaine\nom_utilisateur ou user_name@domain.com.
   - **Mot de passe** : mot de passe associé à l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour les connexions à distance doit avoir un mot de passe associé.
   - **Nom convivial** : si vous utilisez le même compte d’utilisateur avec des mots de passe différents, définissez un nom convivial pour distinguer ces comptes d’utilisateurs.
4. Appuyez sur **Enregistrer**, puis sur **Paramètres**.

## <a name="customize-your-display-resolution"></a>Personnaliser la résolution de votre périphérique d’affichage
Vous pouvez spécifier la résolution du périphérique d’affichage de votre session Bureau à distance.

1. Dans le Centre de connexion, cliquez sur **Préférences**.
2. Cliquez sur **Résolution**. 
3. Cliquez sur **+** .
4. Entrez une hauteur et une largeur de résolution, puis cliquez sur **OK**.

Pour supprimer la résolution, sélectionnez-la, puis cliquez sur **-** .

**Les périphériques d’affichage ont des espaces séparés** : si vous exécutez Mac OS X 10.9 et que vous avez désactivé **Les périphériques d’affichage ont des espaces séparés** dans Mavericks (**Préférences système > Contrôle des missions**), vous devez configurer ce paramètre dans le client Bureau à distance en utilisant la même option.

### <a name="drive-redirection-for-remote-resources"></a>Redirection de lecteur pour les ressources distantes
La redirection de lecteur est prise en charge pour les ressources distantes, de sorte que vous pouvez enregistrer les fichiers créés avec une application distante localement sur votre Mac. Le dossier redirigé est toujours votre répertoire personnel affiché en tant que lecteur réseau dans la session à distance.

> [!NOTE]
> Pour utiliser cette fonctionnalité, l’administrateur doit définir les paramètres appropriés sur le serveur.


## <a name="use-a-keyboard-in-a-remote-session"></a>Utiliser un clavier dans une session à distance

Les dispositions du clavier Windows diffèrent des dispositions du clavier Mac. 

- La touche Commande du clavier Mac correspond à la touche Windows.
- Pour effectuer des actions qui utilisent la touche Commande sur le Mac, vous devez utiliser la touche Ctrl (Contrôle) dans Windows. Par exemple : Copier = Ctrl + C.
- Les touches de fonction peuvent être activés dans la session en pressant également la touche Fn (par exemple : Fn + F1).
- La touche Alt située à droite de la barre d’espace sur le clavier Mac correspond à la touche Alt Gr/Alt droite dans Windows.

Par défaut, la session à distance utilisera les mêmes paramètres régionaux de clavier que le système d’exploitation sur lequel vous exécutez le client. Si votre Mac est en cours d’exécution sur un système d’exploitation EN-US (Anglais des États-Unis), cette langue sera aussi utilisée pour les sessions à distance. Si les paramètres régionaux du clavier du système d’exploitation ne sont pas utilisés, vérifiez le paramètre du clavier sur le PC distant et modifiez ce paramètre manuellement. Si vous souhaitez obtenir davantage d’informations sur les claviers et les paramètres régionaux, veuillez consulter le [Forum aux questions du client Bureau à distance](remote-desktop-client-faq.md).


## <a name="support-for-remote-desktop-gateway-pluggable-authentication-and-authorization"></a>Prise en charge de l’autorisation et de l’authentification enfichables de la passerelle des services Bureau à distance

Windows Server 2012 R2 a introduit la prise en charge d’une nouvelle méthode d’authentification, l’autorisation et l’authentification enfichables de la passerelle des services Bureau à distance, qui offre plus de souplesse pour les routines d’authentification personnalisée. Vous pouvez maintenant utiliser ce modèle d’authentification avec le client Mac. 

> [!IMPORTANT]
> Les modèles d’authentification et d’autorisation personnalisés avant Windows 8.1 ne sont pas pris en charge, bien que l’article ci-dessus les évoque.

Pour en savoir plus sur cette fonctionnalité, veuillez consulter la page [http://aka.ms/paa-sample](http://aka.ms/paa-sample).


> [!TIP]
> Vos questions et vos commentaires sont toujours les bienvenus. Toutefois, merci de ne pas utiliser la fonctionnalité de commentaire qui figure à la fin de cet article pour nous envoyer une demande d’aide. Veuillez plutôt accéder au [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrez un nouveau fil de discussion. Vous souhaitez nous suggérer une fonctionnalité ? N’hésitez pas à utiliser le [forum UserVoice pour le client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android) afin de nous en faire part.


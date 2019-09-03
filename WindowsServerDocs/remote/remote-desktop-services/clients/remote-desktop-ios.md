---
title: Bien démarrer avec le client iOS
description: Découvrez comment configurer le client Bureau à distance pour iOS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 03ec5a3d-d3f2-4afd-9405-ae58b6ecc91c
author: lizap
manager: dongill
ms.author: elizapo
date: 08/27/2019
ms.localizationpriority: medium
ms.openlocfilehash: f5a0808148068282c218343a923b357267e724e5
ms.sourcegitcommit: 51eaab0f860312d97293fd90f3e632e7caee3df1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70150940"
---
# <a name="get-started-with-the-ios-client"></a>Bien démarrer avec le client iOS

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Le client Bureau à distance sur iOS vous permet d’accéder à des applications, ressources et bureaux Windows à partir de votre appareil iOS (iPhone et iPad).

Aidez-vous des informations suivantes pour démarrer. Consultez le [Forum aux questions (FAQ)](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Vous êtes curieux de découvrir les nouvelles versions du client iOS ? Consultez [Nouveautés du Bureau à distance sur iOS](ios-whatsnew.md).
> - Le client iOS prend en charge les appareils exécutant iOS 6.x ou une version ultérieure.

## <a name="get-the-remote-desktop-beta-client-and-start-using-it"></a>Obtenir le client Bureau à distance bêta et commencer à l’utiliser
Le client iOS bêta disponible aujourd’hui via Apple TestFlight prend en charge les connexions aux ressources Windows Virtual Desktop.

### <a name="download-the-remote-desktop-ios-beta-client-from-apple-testflight"></a>Télécharger la version bêta du client Bureau à distance bêta pour iOS auprès d’Apple TestFlight
Voici comment configurer le client Bureau à distance Beta sur votre appareil iOS :

1. Installez l’application [Apple TestFlight](https://apps.apple.com/us/app/testflight/id899247664) sur votre appareil iOS.
2. Sur votre appareil iOS, ouvrez un navigateur et accédez à [aka.ms/rdiosbeta](https://aka.ms/rdiosbeta).
3. Sous l’étiquette **Step 2 Join the Beta** (Étape 2, Participez à la version bêta), sélectionnez **Start Testing** (Commencer à tester).
4. Quand vous êtes redirigé vers l’application TestFlight, sélectionnez **Accept** (Accepter), puis **installez** le client.

### <a name="add-a-connection-to-a-pc"></a>Ajouter une connexion à un PC
Pour créer une connexion à distance avec un PC :

1. Dans le Centre de connexion, appuyez sur **+** , puis sur **Add PC** (Ajouter un PC).
2. Entrez le nom du PC distant dans **PC Name** (Nom du PC). Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations du port au nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
3. Sélectionnez le **User Account** (Compte d’utilisateur) à utiliser pour accéder au PC distant.
   - Sélectionnez **Ask Every Time** (Demander à chaque fois) pour que le client demande vos informations d’identification chaque fois que vous vous connectez au PC distant.
   - Sélectionnez **Add User Account** (Ajouter un compte d’utilisateur) pour enregistrer un compte que vous utilisez fréquemment, afin de ne pas avoir à entrer les informations d’identification chaque fois que vous vous connectez. Suivez [ces instructions](#manage-your-user-accounts) pour gérer vos comptes d’utilisateur.
4. Vous pouvez également définir les paramètres facultatifs suivants :
   - Dans **Friendly Name** (Nom convivial), vous pouvez entrer un nom facile à mémoriser pour le PC auquel vous vous connectez.
   - **Admin Mode** (Mode administrateur) vous permet de vous connecter à une session d’administration sur le PC distant.
   - **Swap Mouse Buttons** (Permuter les boutons de la souris) permute les commandes envoyées par les gestes droit et gauche de la souris. Idéal pour les utilisateurs gauchers.
   - **Gateway** (Passerelle) est la passerelle Bureau à distance que vous allez utiliser pour vous connecter à un ordinateur à partir d’un réseau externe. Pour plus d’informations, contactez votre administrateur système.
   - **Sound** (Son) sélectionne l’appareil que votre session à distance utilise pour l’audio. Vous pouvez choisir d’activer le son sur votre appareil local ou sur l’appareil distant, ou de désactiver entièrement le son.
   - **Microphone** active la redirection du microphone. Ce paramètre est désactivé par défaut.
   - **Camera** (Appareil photo) active la redirection de l’appareil photo. Ce paramètre est désactivé par défaut.
   - **Clipboard** (Presse-papiers) active la redirection du Presse-papiers. Ce paramètre est activé par défaut.
   - **Storage** (Stockage) active la redirection du stockage local. Ce paramètre est désactivé par défaut.
5. Sélectionnez **Save** (Enregistrer) pour ajouter la connexion du PC distant.

### <a name="add-remote-resources"></a>Ajouter des ressources distantes
Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés par votre administrateur. Le client iOS prend en charge les ressources publiées à partir de **Services Bureau à distance** et des déploiements de **Windows Virtual Desktop**. Pour ajouter des ressources distantes :

1. Dans le Centre de connexion, appuyez sur **+** , puis sur **Add Workspace** (Ajouter un espace de travail).
2. Entrez la **Feed URL** (URL du flux). Il peut s’agir d’une URL ou d’une adresse e-mail :
   - L’**URL** est l’URL du serveur Accès Bureau à distance par le web, qui vous fournie par votre administrateur. Si vous accédez à des ressources à partir de Windows Virtual Desktop, vous pouvez utiliser `https://rdweb.wvd.microsoft.com`.
   - Si vous prévoyez d’utiliser **Email** (E-mail), entrez votre adresse e-mail dans ce champ. Ceci indique au client de rechercher un serveur Accès Bureau à distance par le web associé à votre adresse e-mail s’il a été configuré par votre administrateur.
3. Appuyez **Next** (Suivant).
4. Spécifiez vos informations de connexion quand vous y êtes invité. Ceci peut varier en fonction du déploiement et peut inclure les éléments suivants :
   - **User Name** (Nom d’utilisateur) : le nom de l’utilisateur qui a l’autorisation d’accéder aux ressources.
   - **Password** (Mot de passe) : le mot de passe associé au nom d’utilisateur.
   - **Additional factor** (Facteur supplémentaire) : il peut vous être demandé si l’authentification a été configurée de cette façon par votre administrateur.
5. Appuyez sur **Enregistrer**.

Les ressources distantes ajoutées seront affichées dans le Centre de connexion.

## <a name="get-the-remote-desktop-client-and-start-using-it"></a>Obtenir le client Bureau à distance et commencer à l’utiliser

### <a name="download-the-remote-desktop-client-from-the-ios-store"></a>Téléchargez le client Bureau à distance à partir du store iOS

Effectuez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil iOS :

1. Téléchargez le client Bureau à distance Microsoft à partir de [iTunes](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8).
2. [Configurez votre PC pour accepter les connexions à distance](remote-desktop-client-faq.md#how-do-i-set-up-a-pc-for-remote-desktop).
3. Ajoutez une [connexion Bureau à distance](#add-a-remote-desktop-connection) ou une [ressource distante](#add-a-remote-resource). Utilisez une connexion pour vous connecter directement à un PC Windows, et une ressource distante pour accéder à un programme RemoteApp, un bureau basé sur une session ou un bureau virtuel publié en local à l’aide de la fonctionnalité Connexions aux programmes RemoteApp et aux services Bureau à distance. Cette fonctionnalité est généralement disponible dans les environnements d’entreprise.

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **+** , puis appuyez sur **Ajouter un PC ou un serveur**.
2. Entrez les informations suivantes pour la connexion Bureau à distance :
   - **Nom du PC** : nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations du port au nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nom d’utilisateur** : nom d’utilisateur à spécifier pour accéder au PC distant. Vous pouvez utiliser les formats suivants : *nom_utilisateur*, *domaine\nom_utilisateur* ou `user_name@domain.com`. Vous pouvez également spécifier si l’utilisateur est invité à entrer un nom d’utilisateur et un mot de passe.
3. Vous pouvez aussi définir les options supplémentaires suivantes :
   - **Nom convivial (facultatif)**  : nom facile à mémoriser pour le PC auquel vous vous connectez. Vous pouvez choisir n’importe quelle chaîne, mais si vous ne spécifiez pas de nom convivial, le nom du PC est affiché.
   - **Passerelle (facultatif)**  : passerelle Bureau à distance par laquelle vous voulez vous connecter aux bureaux virtuels, programmes RemoteApp et bureaux basés sur une session dans un réseau interne d’entreprise. Demandez les informations sur la passerelle à votre administrateur système.
   - **Son** : sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir d’activer le son sur les appareils locaux ou l’appareil distant, ou de désactiver entièrement le son.
   - **Permuter les boutons de la souris** : chaque fois qu’un mouvement de souris envoie une commande avec le bouton gauche de la souris, il envoie la même commande avec le bouton droit de la souris à la place. C’est nécessaire si le PC distant est configuré pour utiliser le mode gaucher de la souris.
   - **Mode Administrateur** : permet de vous connecter à une session d’administration sur un serveur exécutant Windows Server 2003 ou une version ultérieure.
4. Appuyez sur **Enregistrer**.

Vous devez modifier ces paramètres ? Appuyez longuement sur le bureau à modifier, puis appuyez sur l’icône des paramètres.

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante

Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés à l’aide de la fonctionnalité Connexions aux programmes RemoteApp et aux services Bureau à distance.

- L’URL affiche le lien vers le serveur d’accès Web des services Bureau à distance qui vous donne accès aux connexions RemoteApp et Bureau à distance.
- Les connexions RemoteApp et Bureau à distance configurées sont affichées.

Pour ajouter une ressource distante :

1. Dans l’écran du Centre de connexion, appuyez sur **+** , puis appuyez sur **Aouter des ressources distantes**.
2. Entrez les informations appropriées pour la ressource distante :
   - **URL de flux** : URL du serveur d’accès web des services Bureau à distance. Vous pouvez également entrer votre compte e-mail professionnel dans ce champ : cela indique au client de rechercher le serveur d’accès Web des services Bureau à distance qui est associé à votre adresse e-mail.
   - **Nom d’utilisateur** : nom d’utilisateur à spécifier pour le serveur d’accès web des services Bureau à distance auquel vous vous connectez.
   - **Mot de passe** : mot de passe à spécifier pour le serveur d’accès Web des services Bureau à distance auquel vous vous connectez.
3. Appuyez sur **Enregistrer**.

Les ressources distantes ajoutées seront affichées dans le Centre de connexion.

## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Quand vous vous connectez à un bureau ou à des ressources distantes, vous pouvez enregistrer les comptes d’utilisateur pour les resélectionner ultérieurement.

Pour créer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Settings** (Paramètres), puis appuyez sur **User Accounts** (Comptes d’utilisateur).
2. Appuyez sur **Ajouter un compte d'utilisateur**.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** : nom d’utilisateur à enregistrer pour l’utiliser avec une connexion à distance. Entrez le nom d’utilisateur dans un de ces formats : nom_utilisateur, domaine\nom_utilisateur ou user_name@domain.com.
   - **Mot de passe** : mot de passe associé à l’utilisateur spécifié. Un mot de passe doit être associé à chaque compte d’utilisateur que vous voulez enregistrer pour les connexions à distance.
4. Appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Settings** (Paramètres), puis appuyez sur **User Accounts** (Comptes d’utilisateur).
2. Sélectionnez le compte que vous souhaitez supprimer.
3. Appuyez sur **Supprimer**.   

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle Bureau à distance vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer les passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le Centre de connexion, appuyez sur **Settings**(Paramètres) > **Gateways** (Passerelles).
2. Appuyez sur **Ajouter ou supprimer la passerelle des services Bureau à distance**.
3. Entrez les informations suivantes :
   - **Nom du serveur** : nom de l’ordinateur que vous souhaitez utiliser comme passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
   - **Nom d’utilisateur** : nom d’utilisateur et mot de passe à spécifier pour la passerelle Bureau à distance à laquelle vous vous connectez. Vous pouvez également sélectionner **Utiliser les informations d’identification de la connexion** si vous préférez garder les mêmes nom d’utilisateur et mot de passe que ceux utilisés pour la connexion Bureau à distance.

## <a name="navigate-the-remote-desktop-session"></a>Naviguer dans la session Bureau à distance
Quand vous démarrez une session Bureau à distance, vous disposez d’outils utiles pour naviguer dans la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session Bureau à distance.
2. Si vous êtes invité à vérifier le certificat du bureau à distance, appuyez sur **Accepter**. Vous pouvez choisir d’accepter automatiquement le certificat en faisant glisser l’option **Ne pas me redemander pour les connexions à cet ordinateur** vers **Activé**.

### <a name="connection-bar"></a>Barre de connexion

La barre de connexion vous donne accès à des contrôles de navigation supplémentaires.

- **Contrôle panoramique** : avec le contrôle panoramique, vous pouvez agrandir et déplacer l’écran. Notez que le contrôle panoramique est uniquement disponible avec l’interaction tactile directe.
   - Activer/désactiver le contrôle panoramique : appuyez sur l’icône panoramique dans la barre de connexion pour afficher le contrôle panoramique et zoomer dans l’écran. Appuyez de nouveau sur l’icône panoramique dans la barre de connexion pour masquer le contrôle et réafficher l’écran dans sa résolution d’origine.
   - Utiliser le contrôle panoramique : appuyez longuement sur le contrôle panoramique et faites-le glisser dans la direction où vous souhaitez déplacer l’écran.
   - Déplacer le contrôle panoramique : appuyez longuement deux fois sur le contrôle panoramique pour le déplacer sur l’écran.
- **Nom de la connexion** : le nom de la connexion active est affiché. Appuyez sur le nom de la connexion pour afficher la barre de sélection de session.
- **Clavier** : appuyez sur l’icône du clavier pour afficher ou masquer le clavier. Le contrôle panoramique s’affiche automatiquement quand le clavier est affiché.
- **Déplacer la barre de connexion** : appuyez longuement sur la barre de connexion, puis faites-la glisser vers un nouvel emplacement en haut de l’écran.

### <a name="session-selection"></a>Sélection de session
Il peut y avoir plusieurs connexions actives sur différents PC en même temps. Appuyez sur la barre de connexion pour afficher la barre de sélection de session sur le côté gauche de l’écran. La barre de sélection de session vous permet de voir toutes vos connexions actives et de passer d’une connexion à une autre.

- Vous pouvez basculer entre les applications dans une session active de ressources distantes.

    Une fois que vous êtes connecté aux ressources distantes, vous pouvez basculer entre les applications ouvertes au sein de cette session en appuyant sur le menu de développement et en choisissant l’application souhaitée dans la liste des éléments disponibles.
- Démarrer une nouvelle session

  Vous pouvez démarrer de nouvelles applications ou sessions de bureau à partir de votre connexion active : appuyez sur **Démarrer nouveau**, puis choisissez l’application ou la session dans la liste des éléments disponibles.

- Déconnecter une session

  Pour déconnecter une session, appuyez sur la croix (X) sur le côté gauche de la vignette de la session.

### <a name="command-bar"></a>Barre de commandes

La barre de commandes remplace la barre Utilitaire depuis la version 8.0.1. Vous pouvez basculer d’un mode souris à un autre et revenir au Centre de connexion à partir de la barre de commandes.

## <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utiliser les mouvements d’interaction tactile et les modes souris dans une session à distance

Le client utilise les mouvements d’interaction tactile standard. Vous pouvez également utiliser les mouvements d’interaction tactile pour répliquer les actions de la souris sur le bureau à distance. Les modes souris disponibles sont décrits dans le tableau ci-dessous.

> [!NOTE]
> Sur Windows 8 ou une version ultérieure, les mouvements d’interaction tactile natifs sont pris en charge en mode d’interaction tactile directe. Pour plus d’informations sur les mouvements sur Windows 8, consultez [Interaction tactile : balayer, appuyer, etc](https://windows.microsoft.com/en-US/windows-8/touch-swipe-tap-beyond).

| Mode souris    | Action avec la souris      | Mouvement                                                    |
|---------------|----------------------|------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez avec un doigt                                               |
| Interaction tactile directe  | Clic droit          | Appuyez longuement avec un doigt                                      |
| Pointeur de souris | Clic gauche           | Appuyez avec un doigt                                               |
| Pointeur de souris | Clic gauche et glissement  | Appuyez longuement deux fois avec un doigt, puis faites glisser                    |
| Pointeur de souris | Clic droit          | Appuyez avec deux doigts                                               |
| Pointeur de souris | Clic droit et glissement | Appuyez longuement deux fois avec deux doigts, puis faites glisser                    |
| Pointeur de souris | Roulette de la souris          | Appuyez longuement avec deux doigts, puis faites glisser vers le haut ou le bas                |
| Pointeur de souris | Zoom                 | Resserrez deux doigts pour faire un zoom avant ou écartez les doigts pour effectuer un zoom arrière |

## <a name="supported-input-devices"></a>Périphériques d’entrée pris en charge

Le [client bêta Bureau à distance pour iOS](https://aka.ms/rdiosbeta) prend en charge les souris Swiftpoint GT et ProPoint. Swiftpoint offre une [remise exclusive](https://www.swiftpoint.com/microsoft/) sur le modèle GT aux utilisateurs de cette bêta.

Actuellement, le client iOS prend uniquement en charge les souris Swiftpoint. Consultez la page [Nouveautés du client iOS](ios-whatsnew.md) et l’[App Store iOS](https://aka.ms/rdios) pour avoir des informations sur la prise en charge future d’autres périphériques.

## <a name="use-a-keyboard-in-a-remote-session"></a>Utiliser un clavier dans une session à distance

Vous pouvez utiliser un clavier visuel ou un clavier physique dans votre session à distance.

Sur un clavier visuel, utilisez le bouton sur le bord droit de la barre située au-dessus du clavier pour basculer entre le clavier standard et un autre.

Si le mode Bluetooth est activé pour votre périphérique iOS, le client détecte automatiquement le clavier Bluetooth.

N’oubliez pas que, en raison de certaines limitations sur le système d’exploitation, les touches spéciales comme Ctrl, Option et Fonction n’ont pas le comportement prévu avec un clavier Bluetooth. Les touches suivantes fonctionnent :

- Touches alphanumériques
- Touches de direction
- Tab : la touche Tab fonctionne, mais pas la combinaison Maj+Tab
- Accueil / Pos1 : Alt+Gauche = Accueil
- Fin : Alt+Droite = Fin
- Page précédente : Alt+Haut = Page précédente
- Page suivante : Alt+Bas = Page suivante
- Tout sélectionner : Commande+A = Ctrl+A (Tout sélectionner dans la plupart des programmes)
- Couper : Commande+X = Ctrl+X (Couper dans la plupart des programmes)
- Copier : Commande+C = Ctrl+C (Copier dans la plupart des programmes)
- Coller : Commande+V = Ctrl+V (Coller dans la plupart des programmes)
- Symboles : La combinaison Alt+touches alphanumériques génère des symboles différents selon la langue configurée

> [!TIP]
> Vos questions et vos commentaires sont toujours les bienvenus. Toutefois, merci de ne pas utiliser la fonctionnalité de commentaire qui figure à la fin de cet article pour nous envoyer une demande d’aide. Veuillez plutôt accéder au [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrez un nouveau fil de discussion. Vous souhaitez nous suggérer une fonctionnalité ? N’hésitez pas à utiliser le [forum UserVoice pour le client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android) afin de nous en faire part.

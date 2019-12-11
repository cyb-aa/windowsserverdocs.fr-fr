---
title: Bien démarrer avec le client Android
description: Informations générales sur le client Android.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 12/02/2019
ms.localizationpriority: medium
ms.openlocfilehash: a1349f1181cdf2ead51a263a3ba62c1789c76b3b
ms.sourcegitcommit: cbf0c7c37797c22af989639fac82fc0eee94497f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2019
ms.locfileid: "74700162"
---
# <a name="get-started-with-the-android-client"></a>Bien démarrer avec le client Android

>S’applique à : Android version 4.1 et ultérieure

Le client Bureau à distance pour Android vous permet d’utiliser des bureaux et applications Windows directement à partir de votre appareil Android ou d’un Chromebook qui prend en charge Google Play Store.

Aidez-vous des informations suivantes pour démarrer. Consultez le [Forum aux questions (FAQ)](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Vous êtes curieux de découvrir les nouvelles versions du client Android ? Découvrez les [nouveautés du client Android](android-whatsnew.md).
> - Le client Android prend en charge les appareils exécutant Android version 4.1 et ultérieur ainsi que les appareils Chromebook avec ChromeOS version 53 et ultérieure. Découvrez-en plus sur les applications Android sur Chrome [ici](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="set-up-the-remote-desktop-client-for-android"></a>Configurer le client Bureau à distance pour Android

### <a name="download-the-remote-desktop-client-from-the-google-play-store"></a>Téléchargez le client Bureau à distance à partir de Google Play Store.

Voici comment configurer le client Bureau à distance sur votre appareil Android :

1. Téléchargez le client Bureau à distance Microsoft à partir de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android).
2. Lancez le **client Bureau à distance** à partir de votre liste d’applications.
3. Ajoutez une [connexion Bureau à distance](#add-a-remote-desktop-connection) ou des [ressources distantes](#add-remote-resources). Vous utilisez une connexion pour vous connecter directement à un PC Windows et à des ressources distantes afin d’accéder aux applications et aux postes de travail publiés par un administrateur à votre intention.

> [!NOTE]
> Si vous souhaitez tester de nouvelles fonctionnalités avant leur publication, nous vous recommandons de télécharger notre [client bêta Bureau à distance Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) à partir de Google Play Store.

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Si ce n’est déjà fait, [configurez votre ordinateur pour qu’il accepte les connexions à distance](remote-desktop-allow-access.md).

Pour créer une connexion Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **+** , puis sur **Bureau**.
2. Entrez le nom du PC distant dans **PC name** (Nom du PC). Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations du port au nom du PC (par exemple, MyDesktop:3389 ou 10.0.0.1:3389). Il s’agit du seul champ obligatoire.
3. Sélectionnez le **Nom d’utilisateur** à utiliser pour accéder au PC distant.
   - Sélectionnez **Entrer à chaque fois** pour que le client demande vos informations d’identification chaque fois que vous vous connectez au PC distant.
   - Sélectionnez **Add user account** (Ajouter un compte d’utilisateur) pour enregistrer un compte que vous utilisez fréquemment, afin de ne pas avoir à entrer les informations d’identification chaque fois que vous vous connectez. Pour plus d’informations, consultez [Gérer vos comptes d’utilisateur](#manage-your-user-accounts).
4. Vous pouvez également appuyer sur **Show additional options** (Afficher des options supplémentaires) pour définir les paramètres facultatifs suivants :
   - Dans **Friendly name** (Nom convivial), vous pouvez entrer un nom facile à mémoriser pour le PC auquel vous vous connectez. Si vous ne spécifiez pas de nom convivial, le nom du PC s’affiche à la place.
   - **Gateway** (Passerelle) est la passerelle Bureau à distance que vous allez utiliser pour vous connecter à un ordinateur à partir d’un réseau externe. Pour plus d’informations, contactez votre administrateur système.
   - **Sound** (Son) sélectionne l’appareil que votre session à distance utilise pour l’audio. Vous pouvez choisir d’activer le son sur votre appareil local ou sur l’appareil distant, ou de désactiver entièrement le son.
   - **Customize display resolution** (Personnaliser la résolution d’affichage) définit la résolution de la session à distance. Quand cette option est désactivée, la résolution spécifiée dans les paramètres globaux est utilisée.
   - **Permuter les boutons de la souris** permute les commandes envoyées par les gestes droit et gauche de la souris. Idéal pour les utilisateurs gauchers.
   - **Se connecter à la session d’administrateur** vous permet de vous connecter à une session d’administrateur sur le PC distant.
   - **Redirect local storage** (Rediriger le stockage local) active la redirection du stockage local. Ce paramètre est désactivé par défaut.
5. Quand vous avez terminé, appuyez sur **Enregistrer**.

Vous devez modifier ces paramètres ? Appuyez sur le menu **Autres options** ( **...** ) à côté du nom du bureau, puis appuyez sur **Modifier**.

Vous souhaitez supprimer la connexion ? Là encore, appuyez sur le menu **Autres options** ( **...** ), puis appuyez sur **Supprimer**.

>[!TIP]
> Si vous obtenez l’erreur 0xf07 à cause d’un mot de passe incorrect (« Nous n’avons pas pu vous connecter à l’ordinateur distant, car le mot de passe associé au compte d’utilisateur a expiré »), changez votre mot de passe et réessayez.

### <a name="add-remote-resources"></a>Ajouter des ressources distantes

Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés par votre administrateur. Le client Android prend en charge les ressources publiées à partir de **Services Bureau à distance** et des déploiements de **Windows Virtual Desktop**. Pour ajouter des ressources distantes :

1. Dans le Centre de connexion, appuyez sur **+** , puis appuyez sur **Remote Resource Feed** (Flux de ressources distantes).
2. Entrez la **Feed URL** (URL du flux). Il peut s’agir d’une URL ou d’une adresse e-mail :
   - L’**URL** est le serveur Accès Bureau à distance par le web, qui vous fournie par votre administrateur. Si vous accédez à des ressources à partir de Windows Virtual Desktop, vous pouvez utiliser `https://rdweb.wvd.microsoft.com`.
   - Si vous prévoyez d’utiliser **Email** (E-mail), entrez votre adresse e-mail dans ce champ. Ceci indique au client de rechercher un serveur Accès Bureau à distance par le web associé à votre adresse e-mail s’il a été configuré par votre administrateur.
3. Appuyez **Next** (Suivant).
4. Spécifiez vos informations de connexion quand vous y êtes invité. Ceci peut varier en fonction du déploiement et peut inclure les éléments suivants :
   - Le nom d’utilisateur (**User name**) qui a l’autorisation d’accéder aux ressources.
   - Le mot de passe (**Password**) associé au nom d’utilisateur.
   - **Additional factor** (Facteur supplémentaire) : il peut vous être demandé si l’authentification a été configurée de cette façon par votre administrateur.
5. Quand vous avez terminé, appuyez sur **Enregistrer**.

Les ressources distantes ajoutées seront affichées dans le Centre de connexion.

Pour supprimer des ressources distantes :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) à côté de la ressource distante.
2. Appuyez sur **Supprimer**.
3. Confirmez la suppression.

### <a name="use-a-widget-to-pin-a-saved-desktop-to-your-home-screen"></a>Utiliser un widget pour épingler un bureau enregistré sur votre écran d’accueil

Le client Bureau à distance prend en charge l’épinglage des connexions à votre écran d’accueil à l’aide de la fonctionnalité de widget Android. La méthode d’ajout d’un widget varie selon le type d’appareil Android utilisé et son système d’exploitation. Voici la méthode la plus courante pour ajouter un widget :

1. Appuyez sur **Applications** pour lancer le menu Applications.
2. Appuyez sur **Widgets**.
3. Faites défiler les widgets pour trouver l’icône Bureau à distance avec la description : « Épingler Bureau à distance ».
4. Appuyez longuement sur ce widget Bureau à distance et déplacez-le vers l’écran d’accueil.
5. Quand vous relâchez l’icône, les bureaux à distance enregistrés s’affichent. Choisissez la connexion à enregistrer sur votre écran d’accueil.

Vous pouvez maintenant démarrer la connexion au bureau à distance directement à partir de votre écran d’accueil en appuyant dessus.

> [!NOTE]
> Si vous renommez la connexion au bureau dans le client Bureau à distance, son étiquette épinglée n’est pas mise à jour.

## <a name="manage-general-app-settings"></a>Gérer les paramètres d’application généraux

Pour changer les paramètres d’application généraux, accédez au Centre de connexion et appuyez sur **Paramètres**, puis sur **Général**.

Vous pouvez définir les paramètres généraux suivants :

- **Afficher les aperçus de bureau** vous permet d’afficher l’aperçu d’un bureau dans le Centre de connexion avant de vous y connecter. Ce paramètre est activé par défaut.
- **Pinch to zoom remote session** (Pincer pour effectuer un zoom sur la session à distance) vous permet de faire un zoom en effectuant un mouvement de pincement. Si l’application que vous utilisez par le biais du Bureau à distance prend en charge l’interaction tactile multipoint (introduite dans Windows 8), désactivez cette fonctionnalité.
- Activez **Use scancode input when available** (Utiliser l’entrée de code de touche enfoncée quand elle est disponible) si votre application distante ne répond pas correctement à l’entrée au clavier envoyée en tant que code de touche enfoncée. L’entrée est envoyée en unicode quand l’option est désactivée.
- **Help improve Remote Desktop** (Aider à améliorer Bureau à distance) envoie des données anonymes à Microsoft sur la façon dont vous utilisez Bureau à distance pour Android. Nous nous servons de ces données pour améliorer le client. Pour en savoir plus sur notre politique de confidentialité et sur les types de données que nous recueillons, consultez la [déclaration de confidentialité de Microsoft](https://privacy.microsoft.com/privacystatement). Ce paramètre est activé par défaut.

## <a name="manage-display-settings"></a>Gérer les paramètres d’affichage

Pour changer les paramètres d’affichage, appuyez sur **Paramètres**, puis sur **Affichage** dans le Centre de connexion.

Vous pouvez définir les paramètres d’affichage suivants :

- **Orientation** définit l’orientation par défaut (paysage ou portrait) pour votre session.
  
  >[!NOTE]
  > Si vous vous connectez à un PC exécutant Windows 8 ou version antérieure, l’orientation de la session ne s’adapte pas correctement si l’orientation de l’appareil change. Pour adapter correctement l’affichage client, déconnectez-vous de l’ordinateur, puis reconnectez-vous dans l’orientation que vous souhaitez utiliser. Vous pouvez également garantir une mise à l’échelle correcte en utilisant un PC avec Windows 10 à la place.

- **Résolution** définit la résolution à distance à utiliser globalement pour les connexions Bureau à distance. Si vous avez déjà défini une résolution personnalisée pour une connexion individuelle, cette résolution reste inchangée.
  
  >[!NOTE]
  >Lorsque vous modifiez les paramètres d’affichage, les modifications s’appliquent uniquement aux nouvelles connexions que vous effectuez après avoir modifié le paramétrage. Pour appliquer vos modifications à la session à laquelle vous êtes actuellement connecté, actualisez votre session en vous déconnectant et en vous reconnectant.

## <a name="manage-your-rd-gateways"></a>Gérer vos passerelles Bureau à distance

Une passerelle Bureau à distance vous permet de vous connecter à un ordinateur distant sur un réseau privé à partir de n’importe où sur Internet. Vous pouvez créer et gérer les passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle des services Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **Paramètres**, puis appuyez sur **Passerelles**.
2. Appuyez sur **+** pour ajouter une nouvelle passerelle.
3. Entrez les informations suivantes :
   - Entrez le nom de l’ordinateur que vous souhaitez utiliser comme passerelle dans **Nom du serveur**. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations de port au nom du serveur (par exemple : RDGateway:443 ou 10.0.0.1:443).
   - Sélectionnez le compte d’utilisateur (**User account**) à utiliser pour accéder à la passerelle distante.
     - Sélectionnez **Use desktop user account** (Utiliser le compte d’utilisateur de bureau) pour utiliser les mêmes informations d’identification que celles que vous avez spécifiées pour l’ordinateur distant.
     - Sélectionnez **Add user account** (Ajouter un compte d’utilisateur) pour enregistrer un compte que vous utilisez fréquemment, afin de ne pas avoir à entrer les informations d’identification chaque fois que vous vous connectez. Pour plus d’informations, consultez [Gérer vos comptes d’utilisateur](#manage-your-user-accounts).

Pour supprimer une passerelle des services Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **Paramètres**, puis appuyez sur **Passerelles**.
2. Appuyez longuement sur une passerelle dans la liste pour la sélectionner. Vous pouvez sélectionner plusieurs passerelles à la fois.
3. Appuyez sur la Corbeille pour supprimer la passerelle sélectionnée.

## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Vous pouvez enregistrer les comptes d’utilisateur à utiliser lorsque vous vous connectez à un bureau à distance ou à des ressources distantes.

Pour enregistrer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Paramètres**, puis appuyez sur **Comptes d’utilisateur**.
2. Appuyez sur **+** pour ajouter un nouveau compte d’utilisateur.
3. Entrez les informations suivantes :
   - Le **Nom d’utilisateur** à enregistrer pour l’utiliser avec une connexion à distance. Entrez le nom d’utilisateur dans un de ces formats : nom_utilisateur, domaine\nom_utilisateur ou user_name@domain.com.
   - Le **Mot de passe** associé à l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour les connexions à distance doit avoir un mot de passe associé.
4. Quand vous avez terminé, appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur enregistré :

1. Dans le Centre de connexion, appuyez sur **Paramètres**, puis appuyez sur **Comptes d’utilisateur**.
2. Appuyez longuement sur un compte d’utilisateur dans la liste pour le sélectionner. Vous pouvez sélectionner plusieurs utilisateurs à la fois.
3. Appuyez sur la Corbeille pour supprimer l’utilisateur sélectionné.

## <a name="navigate-the-remote-desktop-session"></a>Naviguer dans la session Bureau à distance

Voici une brève présentation de la façon d’ouvrir et de parcourir votre session Bureau à distance.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur **le nom de votre connexion Bureau à distance** pour démarrer la session.
2. Si vous êtes invité à vérifier le certificat du bureau à distance, appuyez sur **Connexion**. Vous pouvez également sélectionner **Don’t ask me again for connections to this computer** (Ne pas me redemander pour les connexions à cet ordinateur) pour accepter automatiquement le certificat par défaut.

### <a name="connection-bar"></a>Barre de connexion

La barre de connexion vous donne accès à des contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée en haut de l’écran, au milieu. Faites glisser la barre vers la gauche ou la droite pour la déplacer.

- **Contrôle panoramique** : avec le contrôle panoramique, vous pouvez agrandir et déplacer l’écran. Le contrôle panoramique est uniquement disponible pour l’interaction tactile directe.
  - Pour afficher le contrôle panoramique, appuyez sur l’icône panoramique dans la barre de connexion pour afficher le contrôle et zoomer dans l’écran. Appuyez de nouveau sur l’icône panoramique pour masquer le contrôle et réafficher l’écran dans sa taille d’origine.
  - Pour utiliser le contrôle panoramique, appuyez longuement dessus et faites-le glisser dans la direction où vous souhaitez déplacer l’écran.
  - Pour déplacer le contrôle panoramique, appuyez longuement deux fois dessus pour le déplacer sur l’écran.
- **Options supplémentaires** : appuyez sur l’icône des options supplémentaires pour afficher la barre de sélection de session et la barre de commandes.
- **Clavier** : appuyez sur l’icône du clavier pour afficher ou masquer le clavier. Le contrôle panoramique s’affiche automatiquement quand le clavier est affiché.

### <a name="session-selection-bar"></a>Barre de sélection de session

Il peut y avoir plusieurs connexions actives sur différents PC en même temps. Appuyez sur la barre de connexion pour afficher la barre de sélection de session sur le côté gauche de l’écran. La barre de sélection de session vous permet de voir toutes vos connexions actives et de passer d’une connexion à une autre.

Une fois que vous êtes connecté aux ressources distantes, vous pouvez basculer entre les applications au sein de cette session en appuyant sur le menu de développement ( **>** ) et en choisissant l’application souhaitée dans la liste des éléments disponibles.

Pour démarrer une nouvelle session dans votre connexion active, appuyez sur **Démarrer nouveau**, puis choisissez la session dans la liste des éléments disponibles.

Pour déconnecter une session, appuyez sur la croix (**X**) sur le côté gauche de la vignette de la session.

### <a name="command-bar"></a>Barre de commandes

Appuyez sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran. Dans la barre de commandes, vous pouvez basculer entre les modes souris (interaction tactile directe et pointeur de la souris) ou appuyer sur le bouton Accueil pour revenir au Centre de connexion. Vous pouvez également appuyer sur le bouton Retour pour revenir au Centre de connexion. Le fait de revenir au Centre de connexion ne déconnecte pas votre session active.

### <a name="use-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utiliser les mouvements d’interaction tactile et les modes souris dans une session à distance

Le client utilise les mouvements d’interaction tactile standard. Vous pouvez également utiliser les mouvements d’interaction tactile pour répliquer les actions de la souris sur le bureau à distance. Le tableau suivant décrit les mouvements qui correspondent aux actions de la souris dans chaque mode souris.

> [!NOTE]
> Les mouvements d’interaction tactile natifs sont pris en charge en mode d’interaction tactile directe dans Windows version 8 ou ultérieure.

| Mode souris    | Action de la souris         | Mouvement                                                                 |
|---------------|----------------------|-------------------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyer avec un doigt                                                     |
| Interaction tactile directe  | Clic droit          | Appuyer longuement avec un doigt, puis relâcher                              |
| Pointeur de souris | Zoom                 | Utilisez deux doigts et resserrez-les pour faire un zoom arrière, ou écartez-les pour faire un zoom avant. |
| Pointeur de souris | Clic gauche           | Appuyer avec un doigt                                                     |
| Pointeur de souris | Clic gauche et glissement  | Appuyer deux fois longuement avec un doigt, puis faire glisser                          |
| Pointeur de souris | Clic droit          | Appuyer avec deux doigts                                                    |
| Pointeur de souris | Clic droit et glissement | Appuyer deux fois longuement avec deux doigts, puis faire glisser                         |
| Pointeur de souris | Roulette de la souris          | Appuyer longuement avec deux doigts, puis faire glisser vers le haut ou vers le bas                     |

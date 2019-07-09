---
title: Bien démarrer avec le Bureau à distance sur Android
description: Étapes de configuration de base pour le client Bureau à distance sur Android.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 64f038e1-40ec-4c67-938b-72edea49e5d8
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 07/24/2018
ms.localizationpriority: medium
ms.openlocfilehash: b4b188eb8148b2f4e5c6672b07884af8fdcd0c60
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "66446744"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Bien démarrer avec le Bureau à distance sur Android

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Le client Bureau à distance sur Android vous permet d’utiliser des bureaux et applications Windows directement à partir de votre appareil Android.

Aidez-vous des informations suivantes pour démarrer. Consultez le [Forum aux questions (FAQ)](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Vous êtes curieux de découvrir les nouvelles versions du client Android ? Consultez [Nouveautés du Bureau à distance sur Android](android-whatsnew.md).
> Vous pouvez exécuter le client sur des appareils Android 4.1 ou plus récents ainsi que sur les appareils Chromebook avec ChromeOS 53 installé. Découvrez-en plus sur les applications Android sur Chrome [ici](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Obtenir le client Bureau à distance et commencer à l’utiliser

Effectuez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil Android :

1. Téléchargez le client Bureau à distance à partir de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurez votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajoutez une connexion Bureau à distance ou une ressource distante. Utilisez une connexion pour vous connecter directement à un PC Windows, et une ressource distante pour accéder à un programme RemoteApp, un bureau basé sur une session ou un bureau virtuel publié en local. 
4. Créez un widget pour accéder rapidement au Bureau à distance.

> [!NOTE]
> Si vous souhaitez essayer de nouvelles fonctionnalités décrites précédemment, nous vous recommandons de télécharger notre [application bêta du Bureau à distance Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) à partir de Google Play Store. 

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance :

1. Dans le Centre de connexion, appuyez sur **+** , puis sur **Bureau**.
2. Entrez les informations suivantes pour l’ordinateur auquel vous souhaitez vous connecter :
   - **Nom du PC** : nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations du port au nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nom d’utilisateur** : nom d’utilisateur à spécifier pour accéder au PC distant. Les formats suivants sont possibles : *nom_utilisateur*, *domaine\nom_utilisateur* ou <em>user_name@domain.com</em>. Vous pouvez également spécifier si l’utilisateur est invité à entrer un nom d’utilisateur et un mot de passe.
3. Vous pouvez aussi définir les options supplémentaires suivantes :
   - **Nom convivial** : nom facile à mémoriser pour le PC auquel vous vous connectez. Vous pouvez choisir n’importe quelle chaîne, mais si vous ne spécifiez pas de nom convivial, le nom du PC est affiché.
   - **Passerelle** : passerelle Bureau à distance par laquelle vous voulez vous connecter aux bureaux virtuels, programmes RemoteApp et bureaux basés sur une session dans un réseau interne d’entreprise. Demandez les informations sur la passerelle à votre administrateur système.
    Vous devez configurer une passerelle Bureau à distance ?
   - **Son** : sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir d’activer le son sur les appareils locaux ou l’appareil distant, ou de désactiver entièrement le son.
   - **Personnaliser la résolution d’affichage** : activez ce paramètre si vous voulez définir une résolution personnalisée pour une connexion. Quand ce paramètre est désactivé, la résolution appliquée est celle que vous avez définie dans les paramètres généraux de l’application.
   - **Permuter les boutons de la souris** : avec cette option, vous pouvez permuter les fonctions du bouton gauche de la souris sur le bouton droit de la souris. (Cela est particulièrement utile si le PC distant est configuré pour un utilisateur gaucher mais que vous utilisez une souris pour droitier.)
   - **Se connecter à la session admin** : avec cette option, vous pouvez vous connecter à une session de console en vue d’administrer un serveur Windows.
   - **Rediriger vers le stockage local** : monte votre stockage local comme un système de fichiers distant sur un PC distant.
4. Appuyez sur **Enregistrer**.

Vous devez modifier ces paramètres ? Appuyez sur le menu de dépassement ( **...** ) à côté du nom du bureau et appuyez ensuite sur **Modifier**.

Vous souhaitez supprimer la connexion ? Là encore, appuyez sur le menu de dépassement ( **...** ), puis appuyez sur **Supprimer**.

>[!TIP]
> Si vous obtenez l’erreur 0xf07 à cause d’un mot de passe incorrect (« Nous n’avons pas pu vous connecter à l’ordinateur distant, car le mot de passe associé au compte d’utilisateur a expiré »), changez votre mot de passe et réessayez.

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Les ressources distantes peuvent être des programmes RemoteApp, des bureaux basés sur une session et des bureaux virtuels publiés à l’aide de la fonctionnalité Connexions aux programmes RemoteApp et aux services Bureau à distance.

Pour ajouter une ressource distante :

1. Dans l’écran du Centre de connexion, appuyez sur **+** , puis appuyez sur **Flux de ressources distantes**. 
2. Entrez les informations appropriées pour la ressource distante :
   - **Adresse de messagerie ou URL** : URL du serveur d’accès Web des services Bureau à distance. Vous pouvez également entrer votre compte e-mail professionnel dans ce champ : cela indique au client de rechercher le serveur d’accès Web des services Bureau à distance qui est associé à votre adresse e-mail.
   - **Nom d’utilisateur** : nom d’utilisateur à spécifier pour le serveur d’accès Web des services Bureau à distance auquel vous vous connectez.
   - **Mot de passe** : mot de passe à spécifier pour le serveur d’accès Web des services Bureau à distance auquel vous vous connectez.
3. Appuyez sur **Enregistrer**.

Les ressources distantes ajoutées seront affichées dans le Centre de connexion.


Pour supprimer des ressources distantes :

1. Dans le Centre de connexion, appuyez sur le menu de dépassement ( **...** ) à côté de la ressource distante.
2. Appuyez sur **Supprimer**.
3. Confirmez la suppression.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Utiliser des widgets pour épingler un bureau enregistré sur votre écran d’accueil

Les applications Bureau à distance prennent en charge l’épinglage des connexions à votre écran d’accueil à l’aide de la fonctionnalité de widget Android. La méthode d’ajout d’un widget varie selon le type d’appareil Android utilisé et son système d’exploitation. Voici la méthode la plus courante pour ajouter un widget : 

1. Appuyez sur **Applications** pour lancer le menu Applications.
2. Appuyez sur **Widgets**.
3. Faites défiler les widgets pour trouver l’icône Bureau à distance avec la description « Épingler Bureau à distance ».
4. Appuyez longuement sur ce widget Bureau à distance et déplacez-le vers l’écran d’accueil.
5. Quand vous relâchez l’icône, les bureaux à distance enregistrés s’affichent. Choisissez la connexion à enregistrer sur votre écran d’accueil.

Vous pouvez maintenant démarrer la connexion au bureau à distance directement à partir de votre écran d’accueil en appuyant dessus.

> [!NOTE]
> Si vous renommez la connexion au bureau à distance dans l’application Bureau à distance, l’étiquette de ce bureau à distance épinglé n’est pas mise à jour avec le nouveau nom.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle Bureau à distance vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer les passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le Centre de connexion, appuyez sur **Paramètres > Passerelles**. Appuyez sur **+** pour ajouter une nouvelle passerelle.
2. Entrez les informations suivantes :
   - **Nom du serveur** : nom de l’ordinateur que vous souhaitez utiliser comme passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez aussi ajouter les informations de port au nom du serveur (par exemple, **RDGateway:443** ou **10.0.0.1:443**).
   - **Nom d’utilisateur** : nom d’utilisateur et mot de passe à spécifier pour la passerelle Bureau à distance à laquelle vous vous connectez. Vous pouvez également sélectionner **Utiliser le compte d’utilisateur du bureau** si vous préférez garder les mêmes informations d’identification que celles utilisées pour la connexion Bureau à distance.

## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Quand vous vous connectez à un bureau ou à des ressources distantes, vous pouvez enregistrer les comptes d’utilisateur pour les resélectionner ultérieurement. Vous pouvez également définir des comptes d’utilisateur directement sur le client, au lieu d’enregistrer les données utilisateur quand vous vous connectez à un bureau.

Pour créer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Paramètres**, puis appuyez sur **Comptes d’utilisateur**.
2. Appuyez sur **+** pour ajouter un nouveau compte d’utilisateur.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** : nom d’utilisateur à enregistrer pour l’utiliser avec une connexion à distance. Entrez le nom d’utilisateur dans un de ces formats : nom_utilisateur, domaine\nom_utilisateur ou user_name@domain.com.
   - **Mot de passe** : mot de passe associé à l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour les connexions à distance doit avoir un mot de passe associé.
4. Appuyez sur **Enregistrer**.

Pour supprimer un compte d’utilisateur :

1. Dans le Centre de connexion, appuyez sur **Paramètres > Comptes d’utilisateur**.
2. Appuyez longuement sur un compte d’utilisateur dans la liste pour le sélectionner. Vous pouvez sélectionner plusieurs utilisateurs.
3. Appuyez sur la Corbeille pour supprimer l’utilisateur sélectionné.

## <a name="navigate-the-remote-desktop-session"></a>Naviguer dans la session Bureau à distance
Quand vous démarrez une connexion Bureau à distance, vous disposez d’outils utiles pour naviguer dans la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session. 
2. Si vous êtes invité à vérifier le certificat du bureau à distance, appuyez sur **Connexion**. Vous pouvez également sélectionner **Ne pas me redemander pour les connexions à cet ordinateur** pour accepter automatiquement le certificat.

### <a name="manage-global-app-settings"></a>Gérer les paramètres d’application généraux

Vous pouvez définir les paramètres généraux suivants sur votre client Android :

- **Afficher les aperçus de bureau** : vous permet d’afficher l’aperçu d’un bureau dans le Centre de connexion avant de vous y connecter. Par défaut, ce paramètre est **activé**.
- **Pincer pour zoomer** : vous permet de faire un zoom en effectuant un mouvement de pincement. Si l’application que vous utilisez par le biais du Bureau à distance prend en charge l’interaction tactile multipoint (introduite dans Windows 8), ce paramètre doit être **désactivé**.
- **Aidez-nous à améliorer le Bureau à distance** : envoie des données anonymes à Microsoft. Nous nous servons de ces données pour améliorer le client. Pour en savoir plus sur la façon dont nous utilisons ces données personnelles anonymes, consultez la [Déclaration de confidentialité pour le client Bureau à distance](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Par défaut, ce paramètre est **activé**.
- **Affichage** : il y a deux paramètres d’affichage généraux :
  - **Orientation** : définit l’orientation par défaut (paysage ou portrait) pour votre session. 
    >[!NOTE]
    > Si vous vous connectez à un PC avec Windows 8 ou une version antérieure de Windows, l’orientation de la session ne s’adapte pas correctement. La meilleure solution est de vous déconnecter du PC, puis de vous y reconnecter dans l’orientation de votre choix. Mieux encore, mettez à niveau votre PC vers Windows 8.1 ou une version ultérieure.

  - **Résolution** : définit la résolution à utiliser globalement pour les connexions Bureau à distance. Si vous avez déjà défini une résolution personnalisée pour une connexion ou une application individuelle, cette résolution reste inchangée.
    >[!NOTE]
    >Quand vous modifiez un paramètre d’affichage, le nouveau paramètre s’applique uniquement aux nouvelles connexions établies après le changement. Pour voir la modification appliquée dans une session à laquelle vous êtes déjà connecté, déconnectez-vous, puis reconnectez-vous.

### <a name="connection-bar"></a>Barre de connexion

La barre de connexion vous donne accès à des contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée en haut de l’écran, au milieu. Appuyez deux fois sur la barre et faites-la glisser vers la gauche ou la droite pour la déplacer.

- **Contrôle panoramique** : avec le contrôle panoramique, vous pouvez agrandir et déplacer l’écran. Notez que le contrôle panoramique est uniquement disponible avec l’interaction tactile directe.
   - Activer/désactiver le contrôle panoramique : appuyez sur l’icône panoramique dans la barre de connexion pour afficher le contrôle panoramique et zoomer dans l’écran. Appuyez de nouveau sur l’icône panoramique dans la barre de connexion pour masquer le contrôle et réafficher l’écran dans sa résolution d’origine.
   - Utiliser le contrôle panoramique : appuyez longuement sur le contrôle panoramique et faites-le glisser dans la direction où vous souhaitez déplacer l’écran.
   - Déplacer le contrôle panoramique : appuyez longuement deux fois sur le contrôle panoramique pour le déplacer sur l’écran.
- **Options supplémentaires** : appuyez sur l’icône des options supplémentaires pour afficher la barre de sélection de session et la barre de commandes (voir ci-dessous).
- **Clavier** : appuyez sur l’icône du clavier pour afficher ou masquer le clavier. Le contrôle panoramique s’affiche automatiquement quand le clavier est affiché.
- **Déplacer la barre de connexion** : appuyez longuement sur la barre de connexion, puis faites-la glisser vers un nouvel emplacement en haut de l’écran.


### <a name="command-bar"></a>Barre de commandes

Appuyez sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran. Vous pouvez passer d’un mode souris à un autre (interaction tactile directe et pointeur de souris). Utilisez le bouton Accueil pour revenir au Centre de connexion à partir de la barre de commandes. Vous pouvez aussi utiliser le bouton Précédent pour la même action. Votre session active ne sera pas déconnectée. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utiliser les mouvements d’interaction tactile directe et les modes souris dans une session à distance

Le client utilise les mouvements d’interaction tactile standard. Vous pouvez également utiliser les mouvements d’interaction tactile pour répliquer les actions de la souris sur le bureau à distance. Les modes souris disponibles sont décrits dans le tableau ci-dessous.

> [!NOTE]
> Sur Windows 8 ou une version ultérieure, les mouvements d’interaction tactile natifs sont pris en charge en mode d’interaction tactile directe. 

| Mode souris    | Action avec la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez avec un doigt                                                          |
| Interaction tactile directe  | Clic droit          | Appuyez longuement avec un doigt                                                 |
| Pointeur de souris | Zoom                 | Resserrez les deux doigts pour faire un zoom ou écartez les doigts pour effectuer un zoom arrière. |
| Pointeur de souris | Clic gauche           | Appuyez avec un doigt                                                          |
| Pointeur de souris | Clic gauche et glissement  | Appuyez longuement deux fois avec un doigt, puis faites glisser                               |
| Pointeur de souris | Clic droit          | Appuyez avec deux doigts                                                          |
| Pointeur de souris | Clic droit et glissement | Appuyez longuement deux fois avec deux doigts, puis faites glisser                               |
| Pointeur de souris | Roulette de la souris          | Appuyez longuement avec deux doigts, puis faites glisser vers le haut ou le bas                           |

> [!TIP]
> Vos questions et vos commentaires sont toujours les bienvenus. Toutefois, n’envoyez PAS votre demande d’aide par le biais de la fonctionnalité de commentaire qui figure à la fin de cet article. Accédez au [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrez un nouveau thread. Vous avez une suggestion de fonctionnalité à nous faire ? Faites-nous en part via le [forum UserVoice pour le client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

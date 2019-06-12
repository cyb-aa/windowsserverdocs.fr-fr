---
title: Bien démarrer avec le Bureau à distance sur Android
description: Configuration de base comme suit pour le client Bureau à distance pour Android.
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
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446744"
---
# <a name="get-started-with-remote-desktop-on-android"></a>Bien démarrer avec le Bureau à distance sur Android

>S’applique à : Windows 10, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2

Vous pouvez utiliser le client Bureau à distance pour Android pour travailler avec les ordinateurs de bureau et les applications Windows directement à partir de votre appareil Android.

Utilisez les informations suivantes pour commencer. Veillez à consulter le [FAQ](remote-desktop-client-faq.md) si vous avez des questions.

> [!NOTE]
> - Curieux de savoir les nouvelles versions du client Android ? Découvrez [quelles sont les nouveautés pour Bureau à distance sur Android ?](android-whatsnew.md)
> Vous pouvez exécuter le client sur Android 4.1 et les périphériques plus récents ainsi que sur les Chromebooks avec 53 ChromeOS installé. En savoir plus sur les applications Android sur Chrome [ici](https://sites.google.com/a/chromium.org/dev/chromium-os/chrome-os-systems-supporting-android-apps).

## <a name="get-the-rd-client-and-start-using-it"></a>Obtenir le client Bureau à distance et l’utiliser

Suivez ces étapes pour bien démarrer avec le Bureau à distance sur votre appareil Android :

1. Télécharger le client Bureau à distance à partir de [Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android). 
2. [Configurer votre PC pour accepter les connexions à distance](remote-desktop-allow-access.md).
3. Ajouter une connexion Bureau à distance ou une ressource distante. Vous utilisez une connexion pour vous connecter directement à un PC Windows et à une ressource distante à utiliser un programme RemoteApp, un bureau basé sur session, ou un bureau virtuel publiée en local. 
4. Créer un widget afin de pouvoir obtenir rapidement au Bureau à distance.

> [!NOTE]
> Si vous souhaitez déployer de nouvelles fonctionnalités précédemment nous vous recommandons de télécharger notre [bêta de bureau à distance Microsoft](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android.beta) l’application à partir de Google Play store. 

### <a name="add-a-remote-desktop-connection"></a>Ajouter une connexion Bureau à distance

Pour créer une connexion Bureau à distance :

1. Dans, appuyez sur le centre de connexion **+** , puis appuyez sur **Desktop**.
2. Entrez les informations suivantes pour l’ordinateur que vous souhaitez vous connecter :
   - **Nom du PC** – le nom de l’ordinateur. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port pour le nom du PC (par exemple, **MyDesktop:3389** ou **10.0.0.1:3389**).
   - **Nom d’utilisateur** – le nom d’utilisateur à utiliser pour accéder à l’ordinateur distant. Vous pouvez utiliser les formats suivants : *user_name*, *domaine\nom_utilisateur*, ou <em>user_name@domain.com</em>. Vous pouvez également spécifier s’il faut demander un nom d’utilisateur et mot de passe.
3. Vous pouvez également définir des options supplémentaires suivantes :
   - **Nom convivial** – un nom facile à mémoriser pour le PC que vous vous connectez à. Vous pouvez utiliser n’importe quelle chaîne, mais si vous ne spécifiez pas un nom convivial, le nom du PC s’affiche.
   - **Passerelle** – passerelle le Bureau à distance que vous souhaitez utiliser pour se connecter à des bureaux virtuels, des programmes RemoteApp et des ordinateurs de bureau basés sur des sessions sur un réseau d’entreprise interne. Obtenir les informations sur la passerelle de votre administrateur système.
    Vous avez besoin pour configurer une passerelle des services Bureau à distance ?
   - **Son** – sélectionnez l’appareil à utiliser pour l’audio pendant votre session à distance. Vous pouvez choisir de lire un son sur les appareils locaux, le périphérique distant, ou pas du tout.
   - **Personnaliser la résolution d’affichage** -définir une résolution personnalisée pour une connexion en activant ce paramètre. Quand désactiver la résolution est appliqué que vous avez définis dans les paramètres globaux de l’application.
   - **Permuter les boutons de la souris** – Utilisez cette option pour échanger des fonctions du bouton gauche de la souris pour le bouton droit de la souris. (Cela est particulièrement utile si l’ordinateur distant est configuré pour un utilisateur gaucher mais que vous utilisez une souris droite.)
   - **Se connecter à la session d’administration** -Utilisez cette option pour vous connecter à une session de console pour administrer un serveur Windows.
   - **Rediriger vers le stockage local** – votre stockage local est monté comme un système de fichiers à distance sur un ordinateur distant.
4. Appuyez sur **enregistrer**.

Vous avez besoin pour modifier ces paramètres ? Appuyez sur le menu de dépassement de capacité ( **...** ) en regard du nom du bureau et puis appuyez sur **modifier**.

Voulez-vous supprimer la connexion ? Là encore, appuyez sur le menu de dépassement de capacité ( **...** ), puis appuyez sur **supprimer**.

>[!TIP]
> Si vous obtenez l’erreur 0xf07 un mot de passe incorrect (« nous n’avons pas pu se connecter à l’ordinateur distant, car le mot de passe associé au compte d’utilisateur a expiré »), modifier votre mot de passe et réessayez.

### <a name="add-a-remote-resource"></a>Ajouter une ressource distante
Ressources distantes sont des programmes RemoteApp, bureaux basés sur session et des bureaux virtuels publiées à l’aide de connexions RemoteApp et bureau.

Pour ajouter une ressource distante :

1. Dans l’écran de centre de connexion, appuyez sur **+** , puis appuyez sur **le flux de ressources à distance**. 
2. Entrez les informations pour la ressource distante :
   - **Adresse e-mail ou URL** -l’URL du serveur d’accès Web de bureau à distance. Vous pouvez également entrer votre compte de messagerie d’entreprise dans ce champ : cela indique au client pour rechercher le serveur d’accès Web Bureau à distance associé à votre adresse de messagerie.
   - **Nom d’utilisateur** -nom d’utilisateur à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
   - **Mot de passe** -mot de passe à utiliser pour le serveur d’accès Web de bureau à distance que vous vous connectez à.
3. Appuyez sur **enregistrer**.

Les ressources à distance seront affichera dans le centre de connexion.


Pour supprimer les ressources distantes :

1. Dans le centre de connexion, appuyez sur le menu de dépassement de capacité ( **...** ) en regard de la ressource distante.
2. Appuyez sur **supprimer**.
3. Confirmer la suppression.

### <a name="widgets--pin-a-saved-desktop-to-your-home-screen"></a>Épingler des widgets : une session Bureau enregistrée sur votre écran d’accueil

Les applications de bureau à distance prend en charge l’épinglage des connexions à votre écran d’accueil à l’aide de la fonctionnalité de widget Android. La manière que vous ajoutez un widget varie selon le type d’appareil Android que vous utilisez et de son système d’exploitation. Voici la méthode la plus courante d’ajouter un widget : 

1. Appuyez sur **applications** pour lancer le menu d’applications.
2. Appuyez sur **widgets**.
3. Faites défiler via les widgets et recherchez l’icône de bureau à distance avec la description « Bureau à distance de code confidentiel. »
4. Appuyez sur ce widget du Bureau à distance et déplacez-le vers l’écran d’accueil.
5. Lorsque vous relâchez l’icône, vous verrez les bureaux à distance enregistrés. Choisissez la connexion que vous souhaitez enregistrer sur votre écran d’accueil.

Vous pouvez maintenant démarrer la connexion Bureau à distance directement à partir de votre écran d’accueil en appuyant dessus.

> [!NOTE]
> Si vous renommez la connexion de bureau dans l’application de bureau à distance, l’étiquette de ce bureau à distance épinglé ne met pas à jour.

## <a name="connect-to-an-rd-gateway-to-access-internal-assets"></a>Se connecter à une passerelle Bureau à distance pour accéder aux ressources internes

Une passerelle des services Bureau à distance (passerelle RD) vous permet de vous connecter à un ordinateur distant sur un réseau d’entreprise à partir de n’importe où sur Internet. Vous pouvez créer et gérer vos passerelles à l’aide du client Bureau à distance.

Pour configurer une nouvelle passerelle :

1. Dans le centre de connexion, appuyez sur **Paramètres > passerelles**. Appuyez sur **+** pour ajouter une nouvelle passerelle.
2. Entrez les informations suivantes :
   - **Nom du serveur** – le nom de l’ordinateur que vous souhaitez utiliser en tant que passerelle. Cela peut être un nom d’ordinateur Windows, un nom de domaine Internet ou une adresse IP. Vous pouvez également ajouter des informations de port au nom du serveur (par exemple : **RDGateway:443** ou **10.0.0.1:443**).
   - **Nom d’utilisateur** -le nom d’utilisateur et le mot de passe à utiliser pour la passerelle Bureau à distance, vous êtes connecté. Vous pouvez également sélectionner **utiliser le compte d’utilisateur du bureau** à utiliser les informations d’identification que celles utilisées pour la connexion Bureau à distance.

## <a name="manage-your-user-accounts"></a>Gérer vos comptes d’utilisateur

Lorsque vous vous connectez à un bureau ou à distance des ressources, vous pouvez enregistrer les comptes d’utilisateur à sélectionner à nouveau. Vous pouvez également définir des comptes d’utilisateur dans le client lui-même, par opposition à l’enregistrement des données utilisateur lorsque vous vous connectez à un ordinateur de bureau.

Pour créer un nouveau compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **paramètres**, puis appuyez sur **comptes d’utilisateur**.
2. Appuyez sur **+** pour ajouter un nouveau compte d’utilisateur.
3. Entrez les informations suivantes :
   - **Nom d’utilisateur** -le nom de l’utilisateur à enregistrer pour une utilisation avec une connexion à distance. Vous pouvez entrer le nom d’utilisateur dans un des formats suivants : user_name, DOMAINE\nom_utilisateur, ou user_name@domain.com.
   - **Mot de passe** -le mot de passe pour l’utilisateur spécifié. Chaque compte d’utilisateur que vous souhaitez enregistrer pour utiliser des connexions à distance doit avoir un mot de passe associé.
4. Appuyez sur **enregistrer**.

Pour supprimer un compte d’utilisateur :

1. Dans le centre de connexion, appuyez sur **Paramètres > comptes d’utilisateur**.
2. Maintenez sur un compte d’utilisateur dans la liste pour le sélectionner. Vous pouvez sélectionner plusieurs utilisateurs.
3. Appuyez sur la Corbeille pour supprimer l’utilisateur sélectionné.

## <a name="navigate-the-remote-desktop-session"></a>Accédez à la session Bureau à distance
Lorsque vous démarrez une connexion Bureau à distance, il existe des outils que vous pouvez utiliser pour parcourir la session.

### <a name="start-a-remote-desktop-connection"></a>Démarrer une connexion Bureau à distance

1. Appuyez sur la connexion Bureau à distance pour démarrer la session. 
2. Si vous êtes invité à vérifier le certificat pour le Bureau à distance, appuyez sur **Connect**. Vous pouvez également sélectionner **me redemander pour les connexions à cet ordinateur** pour toujours accepter le certificat.

### <a name="manage-global-app-settings"></a>Gérer les paramètres d’application globale

Vous pouvez définir les paramètres globaux suivants dans votre client Android :

- **Afficher les aperçus de bureau** -vous permet d’afficher un aperçu d’un ordinateur de bureau dans le centre de connexion avant de vous connecter à celui-ci. Par défaut, elle est définie **sur**.
- **Pincer pour zoomer** -vous permet d’utiliser des gestes de pincement pour zoomer. Si l’application que vous utilisez le Bureau à distance prend en charge l’interaction tactile multipoint (introduite dans Windows 8), activer ce paramètre **hors**.
- **Aide pour améliorer le Bureau à distance** -envoie des données anonymes à Microsoft. Nous utilisons ces données pour améliorer le client. Vous pouvez en savoir plus sur la façon dont nous utilisons ces données anonymes, privées, consultez le [déclaration de confidentialité de Client Bureau à distance](https://www.microsoft.com/privacystatement/RemoteApp/Default.aspx). Par défaut, ce paramètre est **sur**.
- **Afficher** -il existe deux paramètres globaux de votre affichage :
  - **Orientation** -définit la préférence d’orientation (paysage ou portrait) pour votre session. 
    >[!NOTE]
    > Si vous vous connectez à un PC exécutant Windows 8 ou une version antérieure de Windows, la session n’évolutive pas correctement. Votre meilleure solution consiste à déconnecter de l’ordinateur, puis vous reconnecter à l’orientation que vous souhaitez utiliser. Une meilleure option consiste à mettre à niveau le PC au moins Windows 8.1.

  - **Résolution** -définit la résolution que vous souhaitez utiliser pour les connexions Bureau dans le monde entier. Si vous avez déjà défini une résolution personnalisée pour une application individuelle ou la connexion, qui ne change pas ce paramètre.
    >[!NOTE]
    >Lorsque vous modifiez un des paramètres d’affichage, ils s’appliquent uniquement aux nouvelles connexions à partir de ce point sur. Pour voir la modification dans une session, vous êtes déjà connecté pour vous déconnecter, puis connectez-vous à nouveau.

### <a name="connection-bar"></a>Barre de connexion

La connexion barre vous donne qu'accès aux contrôles de navigation supplémentaires. Par défaut, la barre de connexion est placée dans le milieu en haut de l’écran. Double-clic et faites glisser la barre à gauche ou droite pour le déplacer.

- **Contrôle Panorama**: Le contrôle panoramique permet à l’écran être agrandi et déplacé. Notez que contrôle Panorama est uniquement disponible si vous utilisez l’interaction tactile directe.
   - Activer / désactiver le contrôle Panorama : Appuyez sur l’icône de panoramique dans la barre de connexion pour afficher le contrôle Panoramique et Zoom sur l’écran. Appuyez sur l’icône de panoramique dans la barre de connexion à nouveau à masquer le contrôle et de retourner l’écran à sa résolution d’origine.
   - Utilisez le contrôle Panorama : Appuyez sur le contrôle Panoramique et faites-le glisser dans la direction que vous souhaitez déplacer l’écran.
   - Déplacer le contrôle Panorama : Double cliquez et maintenez le contrôle panoramique pour déplacer le contrôle sur l’écran.
- **Options supplémentaires**: Appuyez sur l’icône des options supplémentaires pour afficher la sélection de session de la barre et commande de la barre (voir ci-dessous).
- **Clavier**: Appuyez sur l’icône de clavier pour afficher ou masquer le clavier. Le contrôle Panorama s’affiche automatiquement lorsque le clavier est affiché.
- **Déplacer la barre de connexion**: Appuyez et maintenez la barre de connexion et puis faites glisser vers un nouvel emplacement en haut de l’écran.


### <a name="command-bar"></a>Barre de commandes

Appuyez sur la barre de connexion pour afficher la barre de commandes sur le côté droit de l’écran. Vous pouvez basculer entre les modes de la souris (interaction tactile directe et pointeur de la souris). Utiliser le bouton Accueil pour revenir au centre de connexion à partir de la barre de commandes. Vous pouvez également utiliser le bouton Précédent pour la même action. Votre session active n’est pas déconnectée. 


### <a name="use-direct-touch-gestures-and-mouse-modes-in-a-remote-session"></a>Utilisation directe touch mouvements et les modes de la souris dans une session à distance

Le client utilise des gestes tactiles standard. Vous pouvez également utiliser des gestes tactiles pour répliquer les actions de la souris sur le Bureau à distance. Les modes de la souris disponibles sont définies dans le tableau ci-dessous.

> [!NOTE]
> Interaction avec Windows 8 ou version ultérieure les mouvements tactiles natif sont pris en charge en mode d’interaction tactile directe. 

| Mode souris    | Opération de la souris      | Mouvement                                                               |
|---------------|----------------------|-----------------------------------------------------------------------|
| Interaction tactile directe  | Clic gauche           | Appuyez sur 1 doigt                                                          |
| Interaction tactile directe  | Clic droit          | 1 doigt maintenez                                                 |
| Pointeur de la souris | Zoom                 | Utilisez les 2 doigts et pincer pour faire un zoom ou déplacer les unes des autres les doigts pour effectuer un zoom arrière. |
| Pointeur de la souris | Clic gauche           | Appuyez sur 1 doigt                                                          |
| Pointeur de la souris | Clic gauche et glissement  | appuyer deux fois 1 doigt et maintenir, puis faites glisser                               |
| Pointeur de la souris | Clic droit          | Appuyez sur 2 doigt                                                          |
| Pointeur de la souris | Clic avec le bouton droit et glissement | 2 double pression du doigt et maintenir, puis faites glisser                               |
| Pointeur de la souris | Roulette de la souris          | 2 doigt appuyez sur et maintenez la touche, puis faites glisser vers le haut ou vers le bas                           |

> [!TIP]
> Questions et vos commentaires sont toujours les bienvenus. Toutefois, ne posez pas une demande de résolution des problèmes à l’aide de la fonctionnalité de commentaire à la fin de cet article. Au lieu de cela, accédez à la [forum du client Bureau à distance](https://social.technet.microsoft.com/forums/windowsserver/en-us/home?forum=winrdc) et démarrer un nouveau thread. Vous avez une suggestion de fonctionnalité ? Dites-nous dans le [forum vocal des utilisateurs client](https://remotedesktop.uservoice.com/forums/272085-remote-desktop-for-android).

---
title: Nouveautés du client macOS
description: En savoir plus sur les dernières modifications apportées au client Bureau à distance pour Mac
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/08/2020
ms.localizationpriority: medium
ms.openlocfilehash: c378d8c4a87b6aa0cf4f6b4f30f3bd5524dbb7a9
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80980851"
---
# <a name="whats-new-in-the-macos-client"></a>Nouveautés du client macOS

Nous mettons régulièrement à jour le [client Bureau à distance pour macOS](remote-desktop-mac.md), en ajoutant de nouvelles fonctionnalités et en corrigeant les problèmes. Vous y trouverez les dernières mises à jour.

Si vous rencontrez des problèmes, vous pouvez toujours nous contacter en sélectionnant **Aide** > **Signaler un problème**.

## <a name="updates-for-version-1039"></a>Mises à jour pour la version 10.3.9

*Date de publication : 06/04/20*

Dans cette version, nous avons apporté des modifications pour améliorer l’interopérabilité avec le [service Bureau virtuel Windows](https://azure.microsoft.com/services/virtual-desktop/). En outre, nous avons inclus les mises à jour suivantes :

- Ctrl + Option + Suppr déclenche maintenant la séquence Ctrl + Alt + Suppr (qui nécessitait auparavant que l’utilisateur appuie sur la touche Fn).
- Correction du modèle de couleurs des notifications en mode clavier pour le mode Clair.
- Scénarios dans lesquels les connexions lancées à l’aide de la propriété de fichier RDP GatewayAccessToken ne fonctionnaient pas.

>[!NOTE]
>Il s’agit de la dernière version qui sera compatible avec macOS 10.12.

## <a name="updates-for-version-1038"></a>Mises à jour pour la version 10.3.8

*Date de publication : 12/02/20*

Voici notre première version de l’année 2020 !

Avec cette mise à jour, vous pouvez basculer entre les modes Scancode (Ctrl + Commande + K) et Unicode (Ctrl + Commande + U) lors d’une entrée clavier. Le mode Unicode permet de taper des caractères étendus à l’aide de la touche Option sur un clavier Mac. Par exemple, sur un clavier QWERTY US Mac, Option + 2 permet d’entrer le symbole de marque déposée (&trade;). Vous pouvez également entrer des caractères accentués en mode Unicode. Par exemple, sur un clavier QWERTY US, si vous appuyez sur Option + E et la touche « A » simultanément, vous entrez le caractère « á » dans votre session à distance.

Les autres mises à jour de cette version sont les suivantes :

- Nettoyage de l’expérience d’actualisation et de l’interface utilisateur de l’espace de travail.
- Correction d’un problème de redirection de carte à puce qui entraînait le blocage de la session à distance au niveau de l’écran de connexion lorsque le message « Vérification de l’état » s’affichait.
- Réduction du temps de création des fichiers temporaires utilisés pour le copier-coller des fichiers placés dans le Presse-papiers.
- Les fichiers temporaires utilisés pour le copier-coller des fichiers du Presse-papiers sont désormais automatiquement supprimés lorsque vous quittez l’application. Il ne revient donc plus à macOS de les supprimer.
- Les actions de signet du PC sont désormais affichées en haut à droite des miniatures.
- Correction de problèmes signalés concernant les données de télémétrie des plantages.

## <a name="updates-for-version-1037"></a>Mises à jour pour la version 10.3.7

*Date de publication : 6/1/20*

Dans notre dernière mise à jour de l’année, nous avons ajusté des sections de code et résolu les comportements suivants :

- La copie d’éléments de la session à distance vers un partage réseau ou un lecteur USB ne crée plus de fichiers vides.
- La spécification d’un mot de passe vide dans un compte d’utilisateur n’entraîne plus une invite de certificat en double.

## <a name="updates-for-version-1036"></a>Mises à jour pour la version 10.3.6

*Date de publication : 6/1/20*

Dans cette version, nous avons résolu un problème qui entraînait la création de fichiers de longueur nulle chaque fois qu’un dossier était copié de la session à distance sur l’ordinateur local par copie-collage de fichiers.

## <a name="updates-for-version-1035"></a>Mises à jour pour la version 10.3.5

*Date de publication : 6/1/20*

Nous avons réalisé cette mise à jour avec l’aide de tous ceux qui ont signalé des problèmes. Dans cette version, nous avons apporté les modifications suivantes :

- Les dossiers redirigés peuvent désormais être marqués en lecture seule pour éviter toute modification de leur contenu dans la session à distance.
- Nous avons résolu l’erreur 0x607 qui se produisait au moment de la connexion dans les scénarios de passerelle Bureau à distance RPC sur HTTPS.
- Résolution des cas où les utilisateurs se voyaient demander leur informations d’identification à deux reprises.
- Résolution des cas où les utilisateurs recevaient l’invite d’avertissement du certificat à deux reprises.
- Ajout de l’heuristique pour améliorer le défilement basé sur trackpad.
- Le client n’affiche plus le groupe « Bureaux enregistrés » s’il n’y a aucun groupe créé par l’utilisateur.
- Mise à jour de l’interface utilisateur des vignettes dans la vue PC.
- Application de correctifs pour résoudre les incidents signalés par la télémétrie de l’application.

> [!NOTE]
> Dans cette version, nous acceptons les commentaires pour le client Mac uniquement par le biais de [UserVoice](https://remotedesktop.uservoice.com/forums/287834-remote-desktop-for-mac).

## <a name="updates-for-version-1034"></a>Mises à jour relatives à la version 10.3.4

*Date de publication : 18/11/19*

Toujours à l’écoute de vos commentaires, nous avons travaillé dur et constitué un ensemble de correctifs de bogues et de mises à jour de fonctionnalités.

- Quand vous vous connectez via une passerelle Bureau à distance avec l’authentification multifacteur, la connexion de la passerelle reste ouverte pour éviter plusieurs invites MFA.
- L’interface utilisateur du client est maintenant entièrement accessible via le clavier avec prise en charge de VoiceOver.
- Maintenant, les fichiers copiés dans le Presse-papiers de la session à distance sont transférés seulement s’ils sont collés dans l’ordinateur local.
- Maintenant, les URL copiées dans le Presse-papiers de la session à distance sont collées correctement dans l’ordinateur local.
- L’accès à distance aux facteurs d’échelle pour prendre en charge les affichages Retina est désormais disponible pour les scénarios multiécrans.
- Résolution d’un problème de compatibilité avec les serveurs Bureau à distance basés sur FreeRDP qui entraînaient des problèmes de connectivité dans les scénarios de redirection.
- Résolution d’un problème de compatibilité de redirection de carte à puce avec les futures versions de Windows 10.
- Résolution d’un problème spécifique à macOS 10.15 où l’espace disponible incorrect était signalé pour les dossiers redirigés.
- Les connexions de PC publiées sont représentées avec une nouvelle icône sous l’onglet Espaces de travail.
- Les « flux » s’appellent maintenant des « espaces de travail » et les « postes de travail » des « PC ».
- Correction d’incohérences et de bogues dans la gestion des comptes d’utilisateur dans l’interface utilisateur des préférences.
- Nombreux correctifs de bogues pour une expérience plus fluide et plus fiable.

## <a name="updates-for-version-1033"></a>Mises à jour relatives à la version 10.3.3

*Date de publication : 18/11/19*

Nous avons regroupé une mise à jour de fonctionnalités et des correctifs de bogues pour la version 10.3.3.

Tout d’abord, nous avons ajouté des valeurs utilisateur par défaut pour désactiver la carte à puce, le Presse-papiers, le microphone, l’appareil photo et la redirection de dossiers :

- ClientSettings.DisableSmartcardRedirection
- ClientSettings.DisableClipboardRedirection
- ClientSettings.DisableMicrophoneRedirection
- ClientSettings.DisableCameraRedirection
- ClientSettings.DisableFolderRedirection

Ensuite, les correctifs de bogues sont les suivants :

- Résolution d’un problème qui provoquait l’absence de détection des redimensionnements de la fenêtre de session programmatique.
- Correction d’un problème où le contenu de la fenêtre de session apparaissait petit lors d’une connexion en mode fenêtre (avec l’affichage dynamique activé).
- Correction du scintillement initial qui se produisait lors d’une connexion à une session en mode fenêtre avec l’affichage dynamique activé.
- Correction de défauts graphiques qui se produisaient lors d’une connexion à Windows 7 après le passage à l’ajustement à la fenêtre avec l’affichage dynamique activé.
- Correction d’un bogue qui entraînait l’envoi d’un nom d’appareil incorrect à la session à distance (avec violation de licence dans certaines applications tierces).
- Résolution d’un problème où les fenêtres d’application distantes occupaient tout l’écran quand elles étaient agrandies.
- Résolution d’un problème où l’interface utilisateur des autorisations d’accès apparaissait sous les fenêtres locales.
- Nettoyage d’un code d’arrêt pour garantir une fermeture plus fiable du client.

## <a name="updates-for-version-1032"></a>Mises à jour relatives à la version 10.3.2

*Date de publication : 18/11/19*

Dans cette version, nous avons résolu un bogue qui faisait passer l’affichage en basse résolution lors d’une connexion à une session.

## <a name="updates-for-version-1031"></a>Mises à jour relatives à la version 10.3.1

*Date de publication : 18/11/19*

Nous avons regroupé des correctifs pour corriger des régressions qui avaient réussi à se glisser dans la version 10.3.0.

- Résolution des problèmes de connectivité avec les serveurs de passerelle Bureau à distance qui utilisaient des clés asymétriques de 4 096 bits.
- Correction d’un bogue qui entraînait le blocage aléatoire du client lors du téléchargement de ressources de flux.
- Correction d’un bogue qui provoquait le plantage du client à l’ouverture.
- Correction d’un bogue qui provoquait le plantage du client lors de l’importation de connexions à partir de Bureau à distance, version 8.

## <a name="updates-for-version-1030"></a>Mises à jour pour la version 10.3.0

*Date de publication : 27/08/19*

Quelques semaines se sont écoulées depuis notre dernière mise à jour, mais nous avons beaucoup travaillé pendant ce temps-là. La version 10.3.0 apporte quelques nouvelles fonctionnalités et de nombreuses corrections de problèmes non visibles.

- La redirection de l’appareil photo est désormais possible lors de la connexion à Windows 10 1809, et à Windows Server 2019 et ultérieur.
- Sur Mojave et Catalina, nous avons ajouté une nouvelle boîte de dialogue qui vous demande l’autorisation d’utiliser le microphone et l’appareil photo pour la redirection de périphérique.
- La procédure d’abonnement à des flux a été réécrite pour la rendre plus simple et plus rapide.
- La redirection du Presse-papiers inclut désormais le format RTF (Rich Text Format).
- Quand vous entrez votre mot de passe, vous avez la possibilité de l’afficher avec la case à cocher « Afficher le mot de passe ».
- Résolution pour les scénarios où la fenêtre de session passait d’un écran à l’autre.
- Le Centre de connexion affiche les icônes de l’application distante en haute résolution (quand elles sont disponibles).
- Cmd+A est mappé à Ctrl+A quand les raccourcis du Presse-papiers Mac sont utilisés.
- Cmd+R actualise désormais tous les flux auxquels vous vous êtes abonné.
- Ajout de nouvelles options de clic secondaire pour développer ou réduire tous les groupes ou flux dans le Centre de connexion.
- Ajout d’une nouvelle option de clic secondaire pour changer la taille des icônes dans l’onglet Flux du Centre de connexion.
- Nouvelle icône d’application simplifiée et claire.

## <a name="updates-for-version-10213"></a>Mises à jour pour la version 10.2.13

*Date de publication : 08/05/2019*

- Correction d’un blocage se produisant lors d’une connexion via une passerelle Bureau à distance.
- Ajout d’un avis de confidentialité à la boîte de dialogue « Ajouter un flux ».

## <a name="updates-for-version-10212"></a>Mises à jour pour la version 10.2.12

*Date de publication : 16/04/2019*

- Résolution des déconnexions aléatoires (avec code d’erreur 0x904) qui survenaient lors d’une connexion via une passerelle Bureau à distance.
- Correction d’un bogue qui vidait la liste des résolutions dans les préférences de l’application après l’installation.
- Correction d’un bogue qui bloquait le client si certaines résolutions étaient ajoutées à la liste des résolutions.
- Traitement de la boucle d’invite d’authentification ADAL lors de la connexion à des déploiements du Bureau virtuel Windows.

## <a name="updates-for-version-10210"></a>Mises à jour pour la version 10.2.10

*Date de publication : 30/03/2019*

- Dans cette version, nous avons traité une instabilité causée par la mise à jour récente 10.14.4 de macOS. Nous avons également corrigé les erreurs de peinture qui s’affichaient lors du décodage des données de codec AVC encodées par un serveur à l’aide du matériel NVIDIA.

## <a name="updates-for-version-1029"></a>Mises à jour pour la version 10.2.9

*Date de publication : 06/03/2019*

- Dans cette version, nous avons corrigé un problème de connectivité de passerelle Bureau à distance qui peut se produire lors de la redirection de serveur.
- Nous avons également traité une régression de passerelle Bureau à distance provoquée par la mise à jour 10.2.8.

## <a name="updates-for-version-1028"></a>Mises à jour pour la version 10.2.8

*Date de publication : 01/03/2019*

- Résolution des problèmes de connectivité qui apparaissaient lorsque vous utilisiez une passerelle Bureau à distance.
- Correction des avertissements de certificat incorrect qui s’affichaient lors de la connexion.
- Traitement de certains cas où la barre de menus et l’accueil disparaissaient inutilement lors du lancement des applications distantes.
- Ajustement du code de redirection du Presse-papiers pour traiter les blocages qui affectaient certains utilisateurs.
- Correction d’un bogue qui entraînait le défilement inutile du centre de connexion lors du lancement d’une connexion.

## <a name="updates-for-version-1027"></a>Mises à jour pour la version 10.2.7

*Date de publication : 06/02/2019*

- Dans cette version, nous avons traité les erreurs de peinture des graphismes (causées par un bogue d’encodage du serveur) qui s’affichaient lors de l’utilisation du mode AVC444.

## <a name="updates-for-version-1026"></a>Mises à jour pour la version 10.2.6

*Date de publication : 28/01/2019*

- Ajout de la prise en charge du codec AVC (420 et 444), disponible lors de la connexion aux versions actuelles de Windows 10.
- Dans le mode Ajuster à la fenêtre, une actualisation de la fenêtre a immédiatement lieu après chaque redimensionnement pour s’assurer que le contenu est affiché selon le niveau d’interpolation approprié.
- Correction d’un bogue de disposition qui entraînait le chevauchement des titres de flux pour certains utilisateurs.
- Nettoyage de l’interface utilisateur des préférences de l’application.
- Amélioration de l’interface utilisateur d’ajout/de modification du bureau.
- Nombreux ajustements de pertinence et finition pour le centre de connexion et les modes Liste pour les bureaux et flux.

>[!NOTE]
>Il existe un bogue dans macOS 10.14.0 et 10.14.1 qui peut entraîner l’utilisation d’une grande quantité d’espace disque par le dossier « .com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA » (imbriqué dans le dossier ~/Library). Pour résoudre ce problème, supprimez le contenu du dossier et effectuez la mise à niveau vers macOS 10.14.2. Notez qu’un effet secondaire de la suppression du contenu du dossier est la suppression des images de capture instantanée attribuée aux Favoris. Ces images seront régénérées lors de la reconnexion au PC distant.

## <a name="updates-for-version-1024"></a>Mises à jour pour la version 10.2.4

*Date de publication : 18/12/2018*

- Ajour de la prise en charge d’un mode sombre pour macOS Mojave 10.14.
- Une option d’importation à partir de Bureau à distance Microsoft 8 s’affiche maintenant dans le centre de connexion dans le cas où il serait vide.
- Traitement de la compatibilité de redirection du dossier avec certaines applications d’entreprise tierces.
- Résolution des problèmes où les utilisateurs recevaient une erreur de passerelle Bureau à distance 0x30000069 en raison de problèmes d’options de secours de protocole de sécurité.
- Correction des problèmes d’affichage progressif rencontrés par certains utilisateurs en mode Ajuster à la fenêtre.
- Correction d’un bogue qui empêchait la copie de fichiers et le collage après avoir copié la dernière version d’un fichier.
- Amélioration du défilement de la souris pour les deltas de défilement plus petits.

## <a name="updates-for-version-1023"></a>Mises à jour pour la version 10.2.3

*Date de publication : 06/11/2018*

- Ajout d’une prise en charge pour le paramètre « remoteapplicationcmdline » du fichier RDP pour les scénarios d’application à distance.
- Le titre de la fenêtre de session inclut désormais le nom du fichier RDP (et le nom du serveur) lorsqu’elle est lancée à partir d’un fichier RDP.
- Correction des problèmes de performances de passerelle Bureau à distance.
- Correction des blocages de passerelle Bureau à distance.
- Résolution des problèmes où la connexion se bloquait lors de la connexion via une passerelle Bureau à distance.
- Meilleure gestion des applications à distance en mode plein écran en masquant intelligemment la barre de menus et l’accueil.
- Correction des scénarios où les applications à distance restent masquées après le lancement.
- Traitement des mises à jour d’affichage lentes lors de l’utilisation du mode « Ajuster à la fenêtre » avec l’accélération matérielle désactivée.
- Gestion des erreurs de création de base de données causées par des autorisations incorrectes lorsque le client démarre.
- Correction d’un problème où le client était constamment bloqué au lancement et ne démarrait pas pour certains utilisateurs.
- Correction d’un scénario où les connexions étaient importées de façon incorrecte en mode plein écran depuis Bureau à distance 8.

## <a name="updates-for-version-1022"></a>Mises à jour pour la version 10.2.2

*Date de publication : 09/10/2018*

- Tout nouveau centre de connexion qui prend en charge les opérations de glisser-déposer, la disposition manuelle des bureaux, les colonnes redimensionnables en mode Liste, le tri par colonne et une gestion des groupes plus simple.
- Le centre de connexion mémorise désormais le dernier sélecteur de vue actif (bureaux ou flux) lors de la fermeture de l’application.
- L’interface utilisateur invitant à renseigner les informations d’identification et les flux ont été révisés.
- Les commentaires de la passerelle Bureau à distance font désormais partie de l’interface utilisateur de l’état de connexion.
- L’importation des paramètres depuis le client version 8 a été améliorée.
- Les fichiers RDP pointant vers les points de terminaison RemoteApp peuvent maintenant être importés dans le centre de connexion.
- Optimisations de l’écran Retina pour les scénarios Bureau à distance de surveillance uniques.
- Prise en charge de la spécification du niveau d’interpolation des graphismes (ce qui affecte l’effet de flou) quand les optimisations Retina ne sont pas utilisées.
- Prise en charge de 256 couleurs pour permettre la connectivité à Windows 2000.
- Correction des détourages des bords de droite et du bas de l’écran lors de la connexion à Windows 7, Windows Server 2008 R2 et versions antérieures.
- La copie d’un fichier local dans Outlook (s’exécutant dans une session distante) ajoute désormais le fichier en tant que pièce jointe.
- Correction d’un problème qui ralentissait les transferts de fichiers basés sur une table de montage si les fichiers proviennent d’un partage de réseau.
- Traitement d’un bogue qui entraînait le blocage d’Excel (s’exécutant dans une session distante) lors de l’enregistrement sous un dossier redirigé.
- Correction d’un problème qui empêchait le signalement d’espace libre pour les dossiers redirigés.
- Correction d’un bogue qui entraînait la consommation de trop de stockage disque par les miniatures sur macOS 10.14.
- Ajout de la prise en charge pour l’application des stratégies de redirection d’appareil de passerelle Bureau à distance.
- Correction d’un problème qui empêchait la fermeture des fenêtres de session lors des déconnexions avec la passerelle Bureau à distance.
- Si l’authentification au niveau du réseau n’est pas appliquée par le serveur, vous êtes désormais redirigé vers l’écran de connexion si votre mot de passe a expiré.
- Résolution des problèmes de performances qui apparaissaient quand un grand nombre de données étaient transférées sur le réseau.
- Corrections de la redirection d’une carte à puce.
- Prise en charge de toutes les valeurs possibles de « EnableCredSspSupport » et des paramètres du fichier RDP « Authentication Level » si la clé d’utilisateur par défaut ClientSettings.EnforceCredSSPSupport (dans le domaine com.microsoft.rdc.macos) est définie sur 0.
- Prise en charge du paramètre de fichier RDP « Prompt for Credentials on Client » quand l’authentification au niveau du réseau n’est pas négociée.
- Prise en charge des connexions basées sur une carte à puce via la redirection de carte à puce à l’invite Winlogon lorsque l’authentification au niveau du réseau n’est pas négociée.
- Correction d’un problème qui empêchait le téléchargement des ressources de flux avec des espaces dans l’URL.

## <a name="updates-for-version-1021"></a>Mises à jour pour la version 10.2.1

*Date de publication : 06/08/2018*

- Connectivité activée pour les PC joints à Azure Active Directory (AAD). Pour vous connecter à un PC joint à AAD, votre nom d’utilisateur doit être dans l’un des formats suivants : « AzureAD\user » ou « AzureAD\user@domain ».
- Traitement de certains bogues qui affectaient l’utilisation de cartes à puce dans une session distante.

## <a name="updates-for-version-1020"></a>Mises à jour pour la version 10.2.0

*Date de publication : 24/07/2018*

- Mises à jour incorporées pour la conformité au RGPD.
- MicrosoftAccount\username@domain est désormais accepté en tant que nom d’utilisateur valide.
- Le partage du Presse-papiers a été réécrit pour être plus rapide et prendre en charge davantage de formats.
- La copie et le collage de textes, d’images ou de fichiers entre les sessions contournent désormais le Presse-papiers de la machine locale.
- Vous pouvez maintenant vous connecter via un serveur de passerelle Bureau à distance avec un certificat non approuvé (si vous acceptez les invites d’avertissement).
- L’accélération matérielle Metal est désormais utilisée (quand elle est prise en charge) pour accélérer l’affichage et optimiser l’utilisation de la batterie.
- Lors de l’utilisation de l’accélération matérielle Metal, nous faisons appel à nos compétences pour que les graphismes de la session aient l’air plus nets.
- Suppression de certaines instances où les fenêtres se bloquaient après avoir été fermées.
- Corrections des bogues qui empêchaient le lancement des programmes RemoteApp dans certains scénarios.
- Correction d’une erreur de synchronisation de canal de passerelle Bureau à distance qui résultait en erreurs 0x204.
- La forme du curseur de la souris se met désormais à jour correctement lorsque vous quittez une session ou une fenêtre RemoteApp.
- Correction d’un bogue de redirection de dossier qui provoquait la perte de données lors des opérations de copie et de collage des dossiers.
- Correction d’un problème de redirection de dossier qui entraînait un signalement incorrect des tailles de dossier.
- Correction d’une régression qui empêchait la journalisation dans un ordinateur joint à AAD à l’aide d’un compte local.
- Correction de bogues qui entraînaient le détourage du contenu de la fenêtre de session.
- Ajout de la prise en charge de certificats de point de terminaison Bureau à distance qui contiennent des clés asymétriques à courbe elliptique.
- Correction d’un bogue qui empêchait le téléchargement de ressources managées dans certains scénarios.
- Traitement d’un problème de détourage avec le centre de connexion épinglé.
- Correction des cases à cocher sous l’onglet Affichage de la fenêtre Ajouter un bureau pour mieux collaborer.
- Le verrouillage des proportions est maintenant désactivé lorsque la modification d’affichage dynamique est appliquée.
- Traitement des problèmes de compatibilité avec l’infrastructure F5.
- Mise à jour de la gestion des mots de passe vides pour vérifier que les messages appropriés sont affichés au moment de la connexion.
- Correction des problèmes de compatibilité de défilement de la souris avec MapInfra Pro.
- Correction de certains problèmes d’alignement dans le centre de connexion lors de l’exécution sur Mojave.

## <a name="updates-for-version-1018"></a>Mises à jour pour la version 10.1.8

*Date de publication : 04/05/2018*

- Ajout de la prise en charge de la modification de la résolution à distance en redimensionnant la fenêtre de session !
- Correction des scénarios où le téléchargement du flux de ressource distante prenait énormément de temps.
- Résolution de l’erreur 0x207 qui pouvait se produire lors de la connexion à des serveurs non corrigés avec la mise à jour pour traiter la correction du chiffrement de l’oracle CredSSP (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Mises à jour pour la version 10.1.7

*Date de publication : 05/04/2018*

- Application de correctifs de sécurité pour incorporer les mises à jour pour traiter la correction du chiffrement de l’oracle CredSSP comme décrite dans CVE-2018-0886.
- Amélioration de l’affichage du curseur de la souris et de l’icône RemoteApp pour traiter les erreurs de peinture signalées.
- Traitement des problèmes où la fenêtre RemoteApp s’affichait derrière le centre de connexion.
- Correction d’un problème qui se produisait lors de la modification des ressources locales après l’importation depuis Bureau à distance 8.
- Vous pouvez maintenant démarrer une connexion en appuyant sur ENTRÉE sur une vignette du bureau.
- Lorsque vous êtes en mode plein écran, CMD + M est maintenant correctement mappé à WIN + M.
- Les fenêtres À propos de, Préférences et Centre de connexion réagissent désormais à CMD+M.
- Vous pouvez maintenant commencer la détection des flux en appuyant sur ENTRÉE dans la page **Adding Remote Resources* (Ajout des ressources distantes).
- Correction d’un problème où un nouveau flux de ressources distantes apparaissait comme étant vide dans le centre de connexion après l’actualisation.

## <a name="updates-for-version-1016"></a>Mises à jour pour la version 10.1.6

*Date de publication : 26/03/2018*

- Correction d’un problème où les fenêtres RemoteApp se réorganisaient elles-mêmes.
- Résolution d’un bogue qui entraînait le blocage de certaines fenêtres RemoteApp derrière leur fenêtre parente.
- Résolution d’un problème de décalage de pointeur de souris qui affectait certains programmes RemoteApp.
- Résolution d’un problème où le démarrage d’une nouvelle connexion plaçait le focus sur une session existante au lieu d’ouvrir une nouvelle fenêtre de session.
- Nous avons corrigé une erreur avec un message d’erreur : vous verrez désormais le message approprié si nous ne trouvons pas votre passerelle.
- Le raccourci Quitter (⌘ + Q) est désormais systématiquement indiqué dans l’interface utilisateur.
- Amélioration de la qualité d’image lorsqu’elle est étendue en mode « Ajuster à la fenêtre ».
- Correction d’une régression qui entraînait l’affichage de multiples instances du dossier d’accueil dans la session distante.
- Mise à jour de l’icône par défaut pour les vignettes de bureau.

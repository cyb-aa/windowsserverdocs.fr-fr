---
title: Nouveautés du client macOS
description: En savoir plus sur les dernières modifications apportées au client Bureau à distance pour Mac
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: heidilohr
manager: daveba
ms.author: helohr
ms.date: 09/11/2019
ms.localizationpriority: medium
ms.openlocfilehash: 092804ea84ba2d0e68eede3a597cb354c53bb8bd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404199"
---
# <a name="whats-new-in-the-macos-client"></a>Nouveautés du client macOS

Nous mettons régulièrement à jour le [client Bureau à distance pour macOS](remote-desktop-mac.md), en ajoutant de nouvelles fonctionnalités et en corrigeant les problèmes. Vous y trouverez les dernières mises à jour.

Si vous rencontrez des problèmes, vous pouvez toujours nous contacter via **Aide > Signaler un problème**.

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
 - Nouvelle icône d’application simplifiée et épurée.

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

- Connectivité activée pour les PC joints à Azure Active Directory (AAD). Pour vous connecter à un PC joint à AAD, votre nom d’utilisateur doit être dans l’un des formats suivants : « AzureAD\user » ou « AzureAD\user@domain ».
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
- Correction des cases à cocher dans la feuille de propriétés d’affichage pour mieux collaborer.
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
- Vous pouvez maintenant commencer la détection des flux en appuyant sur ENTRÉE sur la page **Adding Remote Resources** (Ajout des ressources distantes).
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

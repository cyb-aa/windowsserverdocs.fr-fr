---
title: Nouveautés pour Bureau à distance sur Mac
description: En savoir plus sur les dernières modifications effectuées dans le client Bureau à distance pour Mac
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 03/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: d0cf81f24374e81ca28c2d2cfd83a394e096706c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844790"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Nouveautés du client de bureau à distance sur macOS

Nous mettre à jour régulièrement le [client Bureau à distance pour macOS](remote-desktop-mac.md), en ajoutant de nouvelles fonctionnalités et résolution des problèmes. Découvrez les dernières mises à jour ci-dessous.

Si vous rencontrez des problèmes, vous pouvez toujours nous contacter via **aide > signaler un problème**.

## <a name="updates-for-version-1029"></a>Mises à jour pour la version 10.2.9
*Date de publication : 3/6/2019*

- Dans cette version, que nous avons résolu un problème de connectivité de passerelle Bureau à distance qui peut se produire lors de la redirection de serveur placer.
- Nous avons résolu également une régression de passerelle Bureau à distance provoquée par le 10.2.8 mise à jour.

## <a name="updates-for-version-1028"></a>Mises à jour pour la version 10.2.8
*Date de publication : 3/1/2019*

- Résoudre les problèmes de connectivité qui apparaît lorsque vous utilisez une passerelle Bureau à distance.
- Correction des avertissements de certificat incorrect qui étaient affichées lors de la connexion.
- Résolu certains cas où la barre de menus et la station d’accueil inutilement masquent lors du lancement des applications à distance.
- Retravailler le code de la redirection du Presse-papiers à l’adresse incidents et les blocages qui ont été reproduiront un jour de certains utilisateurs.
- Correction d’un bogue qui a provoqué le centre de connexion inutilement défilement lors du lancement d’une connexion.

## <a name="updates-for-version-1027"></a>Mises à jour pour la version 10.2.7
*Date de publication : 2/6/2019*

- Dans cette version, nous avons résolu mispaints graphics (provoqués par un bogue de codage de serveur) qui s’affiche lors de l’utilisation du mode de AVC444.

## <a name="updates-for-version-1026"></a>Mises à jour pour la version 10.2.6
*Date de publication : 1/28/2019*

- Prise en charge ajoutée pour le codec AVC (420 et 444), disponible lors de la connexion aux versions actuelles de Windows 10.
- Dans l’ajuster à la fenêtre mode, une actualisation de la fenêtre apparaît maintenant immédiatement après un redimensionnement pour vous assurer que le contenu est restitué au niveau de l’interpolation correcte.
- Correction d’un bogue de disposition qui a provoqué des en-têtes de flux se chevaucher pour certains utilisateurs.
- Nettoyé dans l’interface utilisateur de préférences de l’Application.
- Poli l’interface utilisateur de bureau Ajouter/modifier.
- Apporté un grand nombre de finaliser des ajustements pour les vues de vignette et liste de centre de connexion pour les postes de travail et flux.

>[!NOTE]
>Il existe un bogue dans le dossier de macOS 10.14.0 et 10.14.1 qui peut entraîner la «.com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA » (imbriqué dans le dossier ~/Library) à consommer une grande quantité d’espace disque. Pour résoudre ce problème, supprimez le contenu du dossier et mettre à niveau à macOS 10.14.2. Notez qu’un effet secondaire de la suppression du contenu du dossier est que les images de capture instantanée affectés aux signets seront supprimées. Ces images seront régénérés lors de la reconnexion à l’ordinateur distant.

## <a name="updates-for-version-1024"></a>Mises à jour pour la version 10.2.4
*Date de publication : 12/18/2018*

- Prise en charge en mode foncé pour macOS Mojave 10.14.
- Une option pour importer à partir de 8 de bureau à distance Microsoft maintenant apparaît dans le centre de connexion si elle est vide.
- Destinataires de compatibilité de la redirection de dossier avec certaines applications d’entreprise tierces.
- Problèmes résolus dans lequel les utilisateurs ont été obtenir une erreur de passerelle Bureau à distance 0x30000069 en raison de la sécurité problèmes de secours de protocole.
- Problèmes de rendu progressif fixe avec que certains utilisateurs ont rencontré ajusteront à la mode de fenêtre.
- Correction d’un bogue qui empêchait copie d’un fichier et coller à partir de la copie de la dernière version d’un fichier.
- Amélioration en fonction de la souris de défilement pour les deltas de défilement petit.

## <a name="updates-for-version-1023"></a>Mises à jour pour la version 10.2.3
*Date de publication : 11/06/2018*

- Prise en charge pour le paramètre « remoteapplicationcmdline » du fichier RDP pour les scénarios d’application à distance.
- Le titre de la fenêtre de session inclut désormais le nom du fichier RDP (et du nom de serveur) lorsque lancée à partir d’un fichier RDP.
- Correction des problèmes de performances de passerelle signalée Bureau à distance.
- Fixe passerelle Bureau à distance signalée tombe en panne.
- Résolution des problèmes où la connexion se bloquait lors de la connexion via une passerelle Bureau à distance.
- Meilleure gestion des applications à distance plein écran en masquant intelligemment la barre de menus et la station d’accueil.
- Correction des scénarios où les applications à distance est restée masquées après son démarrage.
- Résolu mises à jour de rendu lente lors de l’utilisation de « Ajuster à la fenêtre » avec accélération matérielle désactivée.
- Gérée des erreurs de création de base de données provoquées par des autorisations incorrectes lors du démarrage du client. 
- Correction d’un problème où le client a été constamment se bloque au lancement et ne démarre ne pas pour certains utilisateurs.
- Correction d’un scénario où les connexions ont été importées correctement comme plein écran à partir de 8 de bureau à distance.

## <a name="updates-for-version-1022"></a>Mises à jour pour la version 10.2.2
*Date de publication : 10/09/2018*

- Un tout nouveau centre de connexion qui prend en charge le glisser -déplacer, disposition manuelle des ordinateurs de bureau, les colonnes redimensionnables dans le mode liste, le tri basé sur la colonne et la gestion de groupe plus simple.
- Le centre de connexion contactez-nous mémorise désormais le dernier pivot actif (ordinateurs de bureau ou flux) lorsque la fermeture de l’application.
- Les informations d’identification invitant l’interface utilisateur et les flux ont été remaniés.
- Commentaires de la passerelle Bureau à distance fait désormais partie de l’état qui se connecte l’interface utilisateur.
- Importation des paramètres à partir du client de la version 8 a été améliorée.
- Les fichiers RDP pointant vers les points de terminaison RemoteApp peuvent maintenant importés dans le centre de connexion.
- Optimisations d’affichage retina pour les scénarios de bureau à distance d’analyse unique.
- Prise en charge pour spécifier le niveau d’interpolation de graphiques (ce qui affecte le flou) ne pas avec les optimisations de Retina.
- prise en charge de 256 couleurs pour activer la connectivité à Windows 2000.
- Correction de détourage des bords droit et inférieur de l’écran lors de la connexion à Windows 7, Windows Server 2008 R2 et versions antérieures.
- Copie un fichier local dans Outlook (en cours d’exécution dans une session à distance) maintenant ajoute le fichier comme pièce jointe.
- Correction d’un problème qui a été ralentir de transferts de fichiers basée sur la table de montage, si les fichiers provenant d’un partage réseau.
- Résolu un bogue qui provoquait vers Excel (en cours d’exécution dans une session à distance) ne répond plus lors de l’enregistrement dans un fichier sur un dossier redirigé.
- Correction d’un problème qui ne provoquait aucun espace libre doit être signalée pour les dossiers redirigés.
- Correction d’un bogue qui entraînait des miniatures de consommer trop de ressources stockage sur disque sur macOS 10.14.
- Prise en charge ajoutée pour mettre en œuvre des stratégies de redirection de périphérique de passerelle Bureau à distance.
- Correction d’un problème qui empêchait windows de la session de fermeture lors de la déconnexion d’une connexion à l’aide de la passerelle Bureau à distance.
- Si l’authentification de niveau réseau (NLA) n’est pas appliquée par le serveur, vous seront routées vers l’écran de connexion si votre mot de passe a expiré.
- Résolution des problèmes de performances qui apparaît quand un grand nombre de données a été transféré sur le réseau.
- Correctifs de la redirection de carte à puce.
- Prend en charge pour toutes les valeurs possibles de la « EnableCredSspSupport » et « Niveau d’authentification » paramètres du fichier RDP si la clé de valeur par défaut ClientSettings.EnforceCredSSPSupport utilisateur (dans le domaine com.microsoft.rdc.macos) est définie sur 0.
- Prise en charge pour le paramètre du fichier RDP de « Invite pour les informations d’identification sur Client » lors de l’authentification NLA n’est pas négocié.
- Prise en charge pour la connexion basée sur les cartes à puce par le biais de la redirection de carte à puce à l’invite de Winlogon, lorsque l’authentification NLA n’est pas négociée.
- Correction d’un problème qui empêchait le téléchargement du flux de ressources qui ont des espaces dans l’URL.

## <a name="updates-for-version-1021"></a>Mises à jour pour la version 10.2.1
*Date de publication : 08/06/2018*

- Connectivité est activée pour Azure Active Directory (AAD) joint à un PC. Pour vous connecter à un PC joint à AAD, votre nom d’utilisateur doit être l’un des formats suivants : « AzureAD\user » ou «AzureAD\user@domain».
- Résolu certains bogues qui affectent l’utilisation de cartes à puce dans une session à distance.

## <a name="updates-for-version-1020"></a>Mises à jour pour la version 10.2.0
*Date de publication : 07/24/2018*

- Mises à jour incorporées pour la conformité RGPD.
- MicrosoftAccount\username@domain est désormais accepté en tant qu’un nom d’utilisateur valide.
- Le partage du Presse-papiers a été réécrit pour être plus rapide et prendre en charge plusieurs formats.
- Copiez et collez le texte, des images ou des fichiers entre les sessions maintenant contourne le Presse-papiers de l’ordinateur local.
- Vous pouvez maintenant vous connecter via un serveur de passerelle Bureau à distance avec un certificat non approuvé (si vous acceptez les invites d’avertissement).
- L’accélération matérielle complète est maintenant utilisée (où il est pris en charge) pour accélérer le rendu et d’optimiser l’utilisation de la batterie.
- Lors de l’utilisation de l’accélération de matériel nu que nous tentera de résoudre certains magique pour rendre les graphiques de session n’apparaissent plus nettes.
- Sommes débarrassés de certaines instances où windows seraient subsistent après en cours de fermeture.
- Bogues résolus qui ont été empêchant le lancement des programmes RemoteApp dans certains scénarios.
- Correction d’une erreur de synchronisation de canal de passerelle Bureau à distance qui aboutissait 0x204 erreurs.
- La forme de curseur de souris maintenant met à jour correctement lorsque vous déplacez en dehors d’une session ou d’une fenêtre de RemoteApp.
- Correction d’un bogue de la redirection de dossier qui provoquait la perte de données lorsque la copier et coller des dossiers.
- Correction d’un problème de redirection de dossier qui a provoqué reporting incorrecte des tailles de dossier.
- Correction d’une régression qui empêchait la journalisation dans un ordinateur joint à AAD à l’aide d’un compte local.
- Bogues résolus qui ont été à l’origine la session de contenu de la fenêtre à découper.
- Prise en charge pour les certificats de point de terminaison de bureau à distance qui contiennent les clés asymétriques à courbe elliptique.
- Correction d’un bogue qui empêchait le téléchargement de ressources managées dans certains scénarios.
- Résoudre un problème de découpage auprès du centre de connexion épinglés.
- Fixe les cases à cocher dans la feuille de propriétés d’affichage à mieux collaborer.
- Proportions verrouillage est désormais désactivé lors de la modification de l’affichage dynamique est en vigueur.
- Résolu les problèmes de compatibilité avec l’infrastructure de F5.
- Mise à jour de gestion des mots de passe vides pour vérifier que les messages appropriés sont affichés au moment de se connecter.
- Avec la souris fixe le défilement des problèmes de compatibilité avec MapInfra Pro.
- Correction des problèmes d’alignement dans le centre de connexion lors de l’exécution sur Mojave.

## <a name="updates-for-version-1018"></a>Mises à jour pour la version 10.1.8
*Date de publication : 05/04/2018*

- Prise en charge pour la modification de la résolution à distance en redimensionnant la fenêtre de session !
- Scénarios fixes où la ressource distante flux téléchargement prendrait trop de temps.
- Permet de résoudre l’erreur 0x207 qui peut se produire lors de la connexion aux serveurs ne pas corrigés avec la CredSSP chiffrement oracle correction mise à jour (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Mises à jour pour la version 10.1.7
*Date de publication : 04/05/2018*

- Apporté des correctifs de sécurité pour intégrer les mises à jour de CredSSP chiffrement oracle mise à jour comme décrit dans CVE-2018-0886.
- Meilleur RemoteApp icône et la souris curseur rendu pour résoudre mispaints signalées.
- Résolu les problèmes où RemoteApp windows apparaissaient derrière le centre de connexion.
- Correction d’un problème s’est produite lors de la modification des ressources locales après l’importation à partir de 8 de bureau à distance.
- Vous pouvez maintenant démarrer une connexion en appuyant sur entrée sur une vignette du bureau.
- Lorsque vous êtes en mode plein écran, CMD + M est maintenant correctement mappé à WIN + M.
- Le centre de connexion, préférences et sur windows maintenant répondent aux CMD + M.
- Vous pouvez maintenant commencer la découverte des flux en appuyant sur entrée le **Ajout des ressources distantes** page.
- Correction d’un problème où un nouveau flux de ressources à distance s’affichaient vide dans le centre de connexion jusqu'à une fois que vous avez actualisé.
 
## <a name="updates-for-version-1016"></a>Mises à jour pour la version 10.1.6
*Date de publication : 03/26/2018*

- Correction d’un problème où RemoteApp windows seraient réorganiser eux-mêmes.
- Résolu un bogue qui entraînait des fenêtres de RemoteApp à coincée derrière leur fenêtre parente.
- Résolu un problème de décalage de pointeur de la souris qui affectés certains programmes RemoteApp.
- Correction d’un problème où démarrer une nouvelle connexion a donné le focus à une session existante, au lieu d’ouvrir une fenêtre de nouvelle session.
- Nous avons corrigé une erreur avec un message d’erreur : vous verrez le message approprié si nous ne trouvons pas votre passerelle.
- Le raccourci Quit (⌘ + Q) est désormais systématiquement indiqué dans l’interface utilisateur.
- Amélioration de la qualité d’image lors de l’extension en mode « ajuster à la fenêtre ».
- Correction d’une régression provoquant plusieurs instances du dossier de base s’affichent dans la session à distance.
- Mise à jour de l’icône par défaut pour les vignettes de bureau.

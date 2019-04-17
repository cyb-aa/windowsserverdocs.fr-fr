---
title: Quelles sont les nouveautés pour les services Bureau à distance sur Mac?
description: En savoir plus sur les modifications récentes pour le client Bureau à distance pour Mac
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
ms.openlocfilehash: 11da8bc333da7f44b2732266837d2df216142f85
ms.sourcegitcommit: 971f6538e8d89af84ef50fc8aab2188bdf6f47cb
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/02/2019
ms.locfileid: "9279120"
---
# <a name="whats-new-for-the-remote-desktop-client-on-macos"></a>Nouveautés du client Bureau à distance sur Mac OS

Nous mettre à jour régulièrement le [client Bureau à distance pour Mac OS](remote-desktop-mac.md), ajouter de nouvelles fonctionnalités et de résoudre les problèmes. Regardez les dernières mises à jour ci-dessous.

Si vous rencontrez des problèmes, vous pouvez toujours nous contacter via **aide > rapport un problème**.

## <a name="updates-for-version-10210"></a>Mises à jour pour la version 10.2.10
*Date de publication: 3/30/2019*

- Dans cette version, nous adressé instabilité due à la mise à jour récente macOS 10.14.4. Nous avons également fixe mispaints survenus lors du décodage des données de codec AVC encodées par un serveur à l’aide de matériel NVIDIA.

## <a name="updates-for-version-1029"></a>Mises à jour pour la version 10.2.9
*Date de publication: 3/6/2019*

- Dans cette version, que nous avons résolu un problème de connectivité de passerelle Bureau à distance pouvant se produire lors de la redirection de serveur et même emplacement.
- Nous avons résolu également une régression de passerelle Bureau à distance a provoqué par le 10.2.8 mise à jour.

## <a name="updates-for-version-1028"></a>Mises à jour pour la version 10.2.8
*Date de publication: 1/3/2019*

- Résoudre les problèmes de connectivité exposées lors de l’utilisation d’une passerelle des services Bureau à distance.
- Avertissements de certificat incorrect fixe qui étaient affichés lors de la connexion.
- Destinataires de certains cas où la barre de menus et de la station d’accueil seraient inutilement masquer lors du lancement des applications à distance.
- Retravailler le code de redirection du Presse-papiers adresse incidents et blocages qui ont été se certains utilisateurs.
- Fixe un bogue qui a provoqué le centre de connexion faire défiler inutilement lors du lancement d’une connexion.

## <a name="updates-for-version-1027"></a>Mises à jour pour la version 10.2.7
*Date de publication: 2/6/2019*

- Dans cette version, nous adressé mispaints graphiques (causées par un bogue de codage server) qui s’affichent lorsque l’utilisation du mode AVC444.

## <a name="updates-for-version-1026"></a>Mises à jour pour la version 10.2.6
*Date de publication: 1/28/2019*

- Prise en charge pour le codec AVC (420 et 444), disponible lorsque vous vous connectez à des versions actuelles de Windows 10.
- En ajuster à mode fenêtre, une actualisation de la fenêtre est désormais effectuée immédiatement après un redimensionnement pour vous assurer que le contenu est restitué au niveau de l’interpolation correcte.
- Fixe un bogue de disposition qui a provoqué des en-têtes de flux pour se chevauchent pour certains utilisateurs.
- L’interface utilisateur de préférences Application supprimées.
- Belles et l’interface utilisateur de bureau Ajouter/modifier.
- Mis à un grand nombre de finaliser ajustements pour les vues de vignette et de la liste de centre de connexion pour les postes de travail et de flux.

>[!NOTE]
>Il y a un bogue dans le dossier macOS 10.14.0 et 10.14.1 pouvant entraîner le «.com.microsoft.rdc.application-data_SUPPORT/_EXTERNAL_DATA» (imbriqué profondément dans le dossier ~/Library) pour consommer une grande quantité d’espace disque. Pour résoudre ce problème, supprimez le contenu du dossier, mettre à niveau vers Mac OS 10.14.2. Notez qu’un effet secondaire de la suppression du contenu du dossier est que les images de capture instantanée assignées à signets seront supprimées. Ces images seront régénérés lors de la reconnexion au PC distant.

## <a name="updates-for-version-1024"></a>Mises à jour pour la version 10.2.4
*Date de publication: 12/18/2018*

- Désormais en charge le mode sombre macOS Mojave 10.14.
- Une option pour importer à partir de 8 de bureau à distance Microsoft maintenant s’affiche dans le centre de la connexion si elle est vide.
- Compatibilité de la redirection de dossier dédié avec certaines applications d’entreprise tiers.
- Problèmes résolus dans lequel les utilisateurs ont été obtenir une erreur de passerelle des services Bureau à distance 0x30000069 en raison de la sécurité des problèmes de secours protocole.
- Certains utilisateurs ont rencontré avec des problèmes de rendu progressif fixe ajuster au mode fenêtre.
- Fixe un bogue qui a empêché copie de fichier et coller à partir de la copie de la dernière version d’un fichier.
- Amélioration de la souris défilement pour les deltas courte action de défilement.

## <a name="updates-for-version-1023"></a>Mises à jour pour la version 10.2.3
*Date de publication: 11/06/2018*

- Prise en charge pour le paramètre de fichier RDP «remoteapplicationcmdline» pour les scénarios d’application à distance.
- Le titre de la fenêtre de session inclut désormais le nom du fichier RDP (et du nom de serveur) lors de son lancement à partir d’un fichier RDP.
- Fixe signalée Bureau à distance des problèmes de performances de passerelle.
- Fixe passerelle Bureau à distance signalée se bloque.
- Problèmes résolus dans lequel la connexion se bloquait lors de la connexion par le biais d’une passerelle des services Bureau à distance.
- Meilleure gestion des applications à distance en plein écran en masquant intelligemment la barre de menus et de la station d’accueil.
- Fixe les scénarios où les applications à distance est restées masquées après le lancement.
- Destinataires de mises à jour de rendu lent lors de l’utilisation de «Ajuster à la fenêtre» avec l’accélération matérielle désactivée.
- Gérée des erreurs de création de base de données dues à des autorisations incorrectes lorsque le client démarre. 
- Résout un problème dans lequel le client a été blocage de façon cohérente lors du lancement et ne démarre ne pas pour certains utilisateurs.
- Fixe un scénario dans lequel les connexions ont été incorrectement importées en plein écran à partir de 8 de bureau à distance.

## <a name="updates-for-version-1022"></a>Mises à jour pour la version 10.2.2
*Date de publication: 10/09/2018*

- Un nouveau centre de connexion qui prend en charge glisser -déplacer, disposition manuelle des ordinateurs de bureau, des colonnes redimensionnables en mode d’affichage liste, basée sur la colonne de tri et la gestion des groupes plus simple.
- Le centre de connexion mémorise maintenant le dernier sélecteur de vue actif (postes de travail ou flux) lorsque la fermeture de l’application.
- Les informations d’identification intervention de l’interface utilisateur et les flux ont été ce plan.
- Commentaires de la passerelle des services Bureau à distance fait désormais partie de l’interface utilisateur d’état qui se connecte.
- Importation des paramètres à partir de la version 8 du client a été améliorée.
- Fichiers RDP en pointant sur les points de terminaison RemoteApp peuvent désormais être importés dans le centre de connexion.
- Optimisations d’affichage rétine pour les scénarios de services Bureau à distance de moniteur unique.
- Prise en charge pour spécifier le niveau d’interpolation de graphiques (qui affecte le flou) lorsque vous N'utilisez pas les optimisations de la rétine.
- prise en charge de 256 couleurs pour permettre la connexion à Windows 2000.
- Fixe de découpage des bords droit et inférieur de l’écran lorsque vous vous connectez à Windows 7, Windows Server 2008 R2 et versions antérieures.
- Copie d’un fichier local dans Outlook (en cours d’exécution dans une session à distance) désormais ajoute le fichier en tant que pièce jointe.
- Résout un problème qui a été ralentit les transferts de fichiers basé sur une table de montage si les fichiers provient d’un partage réseau.
- Résolu un bogue qui provoquait se bloque lors de l’enregistrement dans un fichier sur un dossier redirigé vers Excel (en cours d’exécution dans une session à distance).
- Résout un problème qui ne provoquait aucun espace libre doivent être déclarées pour les dossiers redirigés.
- Fixe un bogue qui a provoqué des miniatures consommer trop stockage sur disque sur Mac OS 10.14.
- Prise en charge de mettre en œuvre les stratégies de redirection de périphérique passerelle des services Bureau à distance.
- Résout un problème qui a empêché fermeture de session windows lors de la déconnexion d’une connexion à l’aide de la passerelle des services Bureau à distance.
- Si l’authentification de niveau réseau (NLA) n’est pas appliqué par le serveur, vous seront routées vers l’écran de connexion si votre mot de passe a expiré.
- Résolu des problèmes de performances exposées quand un grand nombre de données a été transférés sur le réseau.
- Correctifs de la redirection de carte à puce.
- Prend en charge toutes les valeurs possibles du «EnableCredSspSupport» et «Niveau d’authentification» paramètres du fichier RDP si la clé de par défaut de l’utilisateur ClientSettings.EnforceCredSSPSupport (dans le domaine com.microsoft.rdc.macos) est définie sur 0.
- Prise en charge pour le paramètre de fichier RDP «Invite pour les informations d’identification sur Client» lors de l’authentification NLA n’est pas négociée.
- Prise en charge pour la connexion de basée sur une carte à puce via la redirection de carte à puce à l’invite Winlogon lors de l’authentification NLA n’est pas négociée.
- Résout un problème qui a empêché le téléchargement des ressources qui ont des espaces de l’URL de flux.

## <a name="updates-for-version-1021"></a>Mises à jour pour la version 10.2.1
*Date de publication: 08/06/2018*

- Connectivité activée à Azure Active Directory (AAD) joints à un PC. Pour se connecter à un AAD joints à un PC, votre nom d’utilisateur doit être dans l’un des formats suivants: «AzureAD\user» ou «AzureAD\user@domain».
- Résolu certains bogues qui affectent l’utilisation de cartes à puce dans une session à distance.

## <a name="updates-for-version-1020"></a>Mises à jour pour la version 10.2.0
*Date de publication: 24/07/2018*

- Mises à jour incorporés pour la conformité du RGPD.
- MicrosoftAccount\username@domainest désormais acceptée comme un nom d’utilisateur valide.
- Le partage du Presse-papiers a été réécrit pour être plus rapide et prennent en charge plusieurs formats.
- Copie et collage de texte, des images ou des fichiers entre les sessions désormais tient pas compte du Presse-papiers de l’ordinateur local.
- Vous pouvez maintenant connecter via un serveur de passerelle Bureau à distance avec un certificat non approuvé (si vous acceptez le message d’avertissement).
- L’accélération matérielle nu est désormais utilisée (si pris en charge) pour accélérer le rendu et optimiser l’utilisation de la batterie.
- Lorsque vous utilisez l’accélération matérielle nu que nous essayez d’utiliser certaines magic pour rendre les graphiques de session sont plus nets.
- Accord rid de certains cas où windows bloquait après la fermeture.
- Les bogues corrigés qui ont été empêche le lancement de programmes RemoteApp dans certains scénarios.
- Corrigé une erreur de synchronisation de canal de passerelle des services Bureau à distance qui a été ce qui entraîne des erreurs de 0x204.
- Le curseur en forme de la souris maintenant met à jour correctement lorsqu’il sort une session ou une fenêtre de RemoteApp.
- Fixe un bogue de redirection de dossiers qui provoquait une perte de données lorsque copier et coller des dossiers.
- Fixe un problème de redirection de dossiers qui a provoqué le signalement incorrect des tailles de dossier.
- Fixe une régression qui a été empêché connectant à un ordinateur joint à AAD à l’aide d’un compte local.
- Les bogues corrigés qui ont été à l’origine de la session de contenu de la fenêtre troncature.
- Prise en charge pour les certificats de point de terminaison de bureau à distance qui contiennent des clés asymétriques à courbe elliptique.
- Fixe un bogue qui a été empêchant le téléchargement de ressources managées dans certains scénarios.
- Résolu un problème de découpage avec le centre de connexion épinglés.
- Fixe les cases à cocher dans la feuille de propriétés d’affichage à mieux collaborer.
- Verrouillage des proportions est désormais désactivé lors de la modification de l’affichage dynamique est en vigueur.
- Résolu des problèmes de compatibilité avec l’infrastructure F5.
- Mise à jour de gestion des mots de passe vides pour garantir qu'un message approprié s’affiche au moment de vous connecter.
- Souris fixe défilement des problèmes de compatibilité avec MapInfra Pro.
- Résolu certains problèmes d’alignement dans le centre de connexion lors du fonctionnement sur Mojave.

## <a name="updates-for-version-1018"></a>Mises à jour pour la version 10.1.8
*Date de publication: 05/04/2018*

- Prise en charge pour la modification de la résolution à distance en redimensionnant la fenêtre de session!
- Fixes scénarios où les ressources distantes flux téléchargement faudrait beaucoup trop de temps.
- Permet de résoudre l’erreur 0x207 qui peut se produire lors de la connexion aux serveurs ne pas corrigés avec la CredSSP chiffrement oracle correction mise à jour (CVE-2018-0886).

## <a name="updates-for-version-1017"></a>Mises à jour pour la version 10.1.7
*Date de publication: 05/04/2018*

- Mis à des correctifs de sécurité pour incorporer des mises à jour de la mise à jour d’oracle CredSSP chiffrement comme décrit dans CVE-2018-0886.
- Améliorée RemoteApp icône et une souris curseur rendu afin de résoudre mispaints signalées.
- Résolu des problèmes où windows RemoteApp apparaissaient derrière le centre de connexion.
- Résout un problème qui se sont produits lorsque vous modifiez les ressources locales après l’importation de 8 de bureau à distance.
- Vous pouvez désormais commencer une connexion en appuyant sur entrée sur une vignette de bureau.
- Lorsque vous êtes en mode plein écran, CMD + M maintenant correctement mappe WIN + M.
- Le centre de connexion, les préférences et à propos de windows est désormais répondent aux CMD + M.
- Vous pouvez désormais commencer à découvrir les flux en appuyant sur entrée sur la page **Ajouter des ressources distantes** .
- Résout un problème dans lequel une nouvelle alimentation à des ressources distantes s’affichaient vide dans le centre de connexion jusqu'à une fois que vous avez actualisé.
 
## <a name="updates-for-version-1016"></a>Mises à jour pour la version 10.1.6
*Date de publication: 26/03/2018*

- Résout un problème dans lequel windows RemoteApp seraient réorganiser eux-mêmes.
- Résolu un bogue qui a provoqué des fenêtres de RemoteApp à êtes bloqué derrière leur fenêtre parente.
- Résolu un problème de décalage de pointeur de la souris qui affectées certains programmes RemoteApp.
- Résout un problème où à partir d’une nouvelle connexion a donné le focus à une session existante, au lieu d’ouvrir une fenêtre de nouvelle session.
- Nous avons corrigé une erreur avec un message d’erreur: affiche le message approprié maintenant si nous ne pouvons pas trouver votre passerelle.
- Le raccourci de quitter (⌘ + Q) est désormais toujours affiché dans l’interface utilisateur.
- Améliorer la qualité d’image lors de l’extension en mode «ajuster à la fenêtre».
- Fixe une régression qui a provoqué plusieurs instances du dossier accueil s’affichent dans la session à distance.
- Mise à jour de l’icône par défaut pour les vignettes bureau.

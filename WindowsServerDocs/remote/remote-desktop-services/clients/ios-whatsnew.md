---
title: Nouveautés du client iOS
description: En savoir plus sur les dernières modifications apportées au client Bureau à distance pour iOS
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: heidilohr
manager: lizross
ms.author: helohr
ms.date: 04/03/2020
ms.localizationpriority: medium
ms.openlocfilehash: 69e33ef5f187672d948b6734a9ae7f68b54b7608
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854652"
---
# <a name="whats-new-in-the-ios-client"></a>Nouveautés du client iOS

Nous mettons régulièrement à jour le [client Bureau à distance pour iOS](remote-desktop-ios.md), en ajoutant de nouvelles fonctionnalités et en corrigeant les problèmes. Cette page liste les dernières mises à jour.

>[!IMPORTANT]
>Dans le cadre de notre engagement à proposer le meilleur client Bureau à distance pour iOS possible, nous apprécions vos commentaires. Vous pouvez signaler les problèmes via **Aide** > **Signaler un problème**.

## <a name="updates-for-version-1005"></a>Mises à jour pour la version 10.0.5

*Date de publication : 09/03/20*

Nous avons rassemblé des correctifs de bogues et des mises à jour de fonctionnalités pour la version 10.0.5. Voici les nouveautés :

- Les fichiers RDP lancés sont désormais automatiquement importés (vous verrez le bouton bascule dans les paramètres généraux).
- Vous pouvez maintenant lancer des fichiers RDP basés sur iCloud qui n’ont pas encore été téléchargés dans l’application Fichiers.
- La session à distance peut maintenant s’étendre sous l’indicateur Accueil des iPhone (vous verrez le bouton bascule dans les paramètres d’affichage).
- Prise en charge de la saisie des caractères composites nécessitant plusieurs frappes de touches (par exemple, « é »).
- Prise en charge du clavier flottant sur l’écran de l’iPad.
- Prise en charge de l’ajustement des propriétés des caméras redirigées à partir d’une session à distance.
- Correction d’un bogue dans le module de reconnaissance de mouvement qui provoquait le blocage du client lors de la connexion à une session à distance.
- Vous pouvez maintenant passer en mode de basculement d’application avec un seul balayage vers le haut (sauf lorsque vous êtes en mode tactile avec la session étendue dans la zone de l’indicateur Accueil).
- L’indicateur Accueil se masque désormais automatiquement lorsque vous êtes connecté à une session à distance et réapparaît quand vous appuyez sur l’écran.
- Ajout d’un raccourci clavier pour accéder aux paramètres de l’application dans le centre de connexion (**Commande + ,** ).
- Ajout d’un raccourci clavier permettant d’actualiser tous les espaces de travail dans le centre de connexion (**Commande + R**).
- Ajout d’un raccourci clavier système pour Échap lors d’une connexion à une session à distance (**Commande + .** ).
- Correction des scénarios dans lesquels le clavier visuel Windows était trop petit lors d’une session à distance.
- Implémentation du focus clavier automatique dans le centre de connexion pour faciliter l’entrée de données.
- Si vous appuyez sur **Entrée** lorsque vous êtes invité à entrer vos informations d’identification, l’invite peut désormais être ignorée et le workflow en cours peut reprendre.
- Correction d’un scénario dans lequel le client plantait lors de l’utilisation des touches Maj + Option + Flèche gauche, haut ou bas.
- Correction d’un plantage qui se produisait lors de la suppression d’un appareil SwiftPoint.
- Correction d’autres plantages signalés par les utilisateurs depuis la dernière version.

Nous aimerions remercier tous ceux qui nous ont signalé des bogues et qui nous ont aidé à diagnostiquer les problèmes.

## <a name="updates-for-version-1004"></a>Mises à jour pour la version 10.0.4

*Date de publication : 03/02/20*

Voici venir une nouvelle mise à jour ! Nous aimerions remercier tous ceux qui nous ont signalé des bogues et qui nous ont aidé à diagnostiquer les problèmes. Voici les nouveautés de cette version :

- L’interface utilisateur de confirmation s’affiche à présent lors de la suppression de comptes d’utilisateur et de passerelles.
- L’interface utilisateur de recherche du centre de connexion a été légèrement retravaillée.
- L’indication du nom d’utilisateur (quand elle existe) s’affiche désormais dans l’invite de demande d’informations d’identification lors d’un lancement à partir d’un fichier RDP ou d’un URI.
- Correction d’un problème lors duquel le clavier visuel étendu s’étendait sous l’encoche iPhone.
- Correction d’un bogue dans lequel les claviers externes cessaient de fonctionner après avoir été déconnectés puis reconnectés.
- Prise en charge de la touche Échap des claviers externes.
- Correction d’un bogue où des caractères anglais s’affichaient lors de l’entrée de caractères chinois.
- Correction d’un bogue où certaines entrées en langue chinoise restaient dans la session à distance après leur suppression.
- Correction d’autres plantages signalés par les utilisateurs depuis la dernière version.

Nous apprécions tous les commentaires que vous nous envoyez via l’App Store, via les commentaires de l’application et par e-mail. Nous visons toujours à améliorer cette application avec chaque version.

## <a name="updates-for-version-1003"></a>Mises à jour pour la version 10.0.3

*Date de publication : 16/01/20*

Nous sommes en 2020 : il est temps pour notre première version de l’année, ce qui signifie nouvelles fonctionnalités et correctifs de bogues. Voici ce que comprend cette mise à jour :

- Prise en charge du lancement des connexions à partir des fichiers RDP et des URI RDP.
- Les en-têtes d’espace de travail sont maintenant réductibles.
- Il est désormais possible d’utiliser le zoom et le panoramique en même temps en mode Pointeur de souris.
- Un appui prolongé en mode Pointeur de souris déclenche désormais un clic droit dans la session à distance.
- Suppression du mouvement Force Touch pour un clic droit en mode Pointeur de souris.
- Dans une session, l’écran du sélecteur prend maintenant en charge la déconnexion, même si aucune application n’est connectée.
- L’abandon interactif est maintenant pris en charge dans l’écran du sélecteur de la session.
- Les PC et les applications ne sont plus automatiquement réorganisés dans l’écran du sélecteur de la session.
- Agrandissement de la zone de test positionnement pour le menu des miniatures du PC.
- La page des paramètres Appareils pris en charge contient maintenant un lien vers les appareils pris en charge.
- Correction d’un bogue qui entraînait l’affichage répété de l’interface utilisateur des autorisations Bluetooth au lancement de certains utilisateurs.
- Correction d’autres plantages signalés par les utilisateurs depuis la dernière version.

## <a name="updates-for-version-1002"></a>Mises à jour relatives à la version 10.0.2

*Date de publication : 20/12/19*

Nous nous efforçons de résoudre les bogues et d’ajouter des fonctionnalités utiles. Nouveautés de cette version :

- Prise en charge des entrées en japonais et en chinois sur les claviers matériels.
- Le mode liste des PC affiche maintenant le nom convivial du compte d’utilisateur associé, s’il en existe un.
- Lors de la première exécution, l’interface utilisateur des autorisations s’affiche désormais correctement en mode Clair.
- Correction d’un plantage qui se produisait chaque fois qu’un utilisateur appuyait simultanément sur les touches Option et Flèche haut (ou bas) sur un clavier matériel.
- Mise à jour de la disposition du clavier visuel utilisée dans l’invite de mot de passe pour faciliter la recherche de la barre oblique inverse.
- Correction d’autres plantages signalés par les utilisateurs depuis la dernière version.

## <a name="updates-for-version-1001"></a>Mises à jour pour la version 10.0.1

*Date de publication : 15/12/19*

Nouveautés de cette version :

- Prise en charge du service Windows Virtual Desktop.
- Mise à jour de l’interface utilisateur du Centre de connexion.
- Mise à jour de l’interface utilisateur de la session.

## <a name="updates-for-version-1000"></a>Mises à jour pour la version 10.0.0

*Date de publication : 13/12/19*

La dernière mise à jour du client Bureau à distance pour iOS remonte à plus d’un an. Nous sommes de retour avec une nouvelle mise à jour très intéressante qui sera suivie de bien d’autres à intervalles réguliers. Voici les nouveautés de la version 10.0.0 :

- Prise en charge du service Windows Virtual Desktop.
- Nouvelle interface utilisateur du Centre de connexion.
- Nouvelle interface utilisateur dans la session permettant de basculer entre PC connectés et applications.
- Nouvelle disposition pour le clavier visuel auxiliaire.
- Prise en charge améliorée des claviers externes.
- Prise en charge des souris Bluetooth SwiftPoint.
- Prise en charge de la redirection du microphone.
- Prise en charge de la redirection du stockage local.
- Prise en charge de la redirection de l’appareil photo (disponible uniquement pour Windows 10 version 1809 ou ultérieure).
- Prise en charge des nouveaux appareils iPhone et iPad.
- Prise en charge des thèmes sombres et clairs.
- Possibilité de contrôler si votre téléphone peut être verrouillé quand il est connecté à une application ou un PC distant.
- Possibilité de réduire la barre de connexion dans la session en appuyant sur le bouton du logo Bureau à distance.

## <a name="updates-for-version-8142"></a>Mises à jour pour la version 8.1.42

*Date de publication : 20/06/2018*

- Améliorations des performances et corrections des bogues.

## <a name="updates-for-version-8141"></a>Mises à jour pour la version 8.1.41

*Date de publication : 28/03/2018*

- Mises à jour pour traiter la correction du chiffrement de l’oracle CredSSP décrite dans CVE-2018-0886.

## <a name="how-to-report-issues"></a>Comment signaler des problèmes

Dans le cadre de notre engagement à proposer la meilleure application possible, nous apprécions vos commentaires. Pour nous signaler des problèmes, accédez à **Paramètres** > **Signaler un problème** dans le client.

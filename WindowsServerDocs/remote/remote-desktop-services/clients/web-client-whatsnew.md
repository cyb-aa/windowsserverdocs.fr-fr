---
title: Nouveautés de client web de bureau à distance ?
description: En savoir plus sur les modifications récentes apportées au client web Bureau à distance
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
ms.date: 05/20/2019
ms.localizationpriority: medium
ms.openlocfilehash: 5be9b05da1e78cc54e12254f43d0f44f7ff65c5d
ms.sourcegitcommit: d888e35f71801c1935620f38699dda11db7f7aad
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66804882"
---
# <a name="whats-new-for-the-remote-desktop-web-client"></a>Nouveautés pour le client web de bureau à distance ?

Nous mettre à jour régulièrement le [client web de bureau à distance](remote-desktop-web-client.md), en ajoutant de nouvelles fonctionnalités et résolution des problèmes. Découvrez les dernières mises à jour ci-dessous.

> [!NOTE]
> Nous avons modifié le système de contrôle de version pour le client web. Depuis la version 1.0.18.0, toutes les versions de publication client web contiendra numéros (au format « W.x.y.z »). Numéros de version pour le client web de bureau à distance se termine toujours par un 0 (par exemple, W.X.Y.0). Chaque version de client de bureau virtuel Windows web change le dernier chiffre jusqu'à ce que la version du client web Bureau à distance suivante (par exemple, 1.0.18.1).

## <a name="updates-for-version-10180"></a>Mises à jour pour la version 1.0.18.0
*Date de publication : 5/14/2019*

- Configuration de la méthode de lancement de ressource ajoutée dans l’onglet Paramètres, permettant aux utilisateurs d’ouvrir des ressources dans le navigateur ou télécharger un fichier .rdp à gérer avec un autre client. Ce paramètre peut être configuré par votre administrateur. Détails concernant les configurations de l’administrateur pour cette fonctionnalité se trouve dans le [documentation d’installation de client web](remote-desktop-web-client-admin.md).
- Couleur fixe problèmes, l’activation de rendu plus vives des couleurs dans votre session à distance.
- Messages d’erreur révisée liés aux erreurs de flux de ressources à distance. 
- Prise en charge plusieurs raccourcis office, telles que collage spécial (Ctrl + Alt + V).
- Raccourci clavier ajouté aux utilisateurs d’appeler la clé de Windows dans la session à distance (Alt + F3)
- Message d’erreur de mise à jour pour les utilisateurs qui tentent de s’authentifier à l’aide d’un mot de passe expiré.
- Flux actualisé l’interface utilisateur sur la page de toutes les ressources.
- Les dialogues qui se chevauchent résolus qui se sont produites pendant la session se reconnecter.
- Dimensionnement d’icône de ressource distante fixe dans la barre des tâches de la ressource.

## <a name="updates-for-version-1011"></a>Mises à jour pour la version 1.0.11
*Date de publication : 2/22/2019*

- Connexion activée pour le service Broker pour les services Bureau à distance sans une passerelle Bureau à distance dans Windows Server 2019.
- Flux par ordre alphabétique (c'est-à-dire RemoteApps tout d’abord, postes de travail deuxième).
- Correction de plusieurs bogues d’accessibilité amélioration de la compatibilité de lecteur d’écran.
- Mise à jour de nos outils de génération.
- Divers correctifs de bogues.

## <a name="updates-for-version-107"></a>Mises à jour pour la version 1.0.7
*Date de publication : 1/24/2019*

- En mode hors connexion sur les réseaux internes est désormais prise en charge.
- Rendu amélioré sur les navigateurs non Microsoft Edge.
- Limite implémentée de nouvelle tentative de récupération de flux tente d’empêcher le déni de service.
- Bogues d’accessibilité fixe, permettant aux utilisateurs présentant un handicap visuel à utiliser le client web.
- Amélioration des messages d’erreur affichés à l’utilisateur pour les erreurs de flux.
- Ajout Ctrl + Alt + Fin (Windows) et fn + contrôle + option + raccourcis de suppression (Mac) pour appeler Ctrl + Alt + Suppr dans l’ordinateur distant.
- Données de télémétrie améliorée pour les événements de panne.
- Amélioration de notre pipeline de build et les outils de génération.
- Divers correctifs de bogues.

## <a name="updates-for-version-101"></a>Mises à jour pour la version 1.0.1
*Date de publication : 10/29/2018*

- Ajout d’une option à **Capture les informations de support** sur la page About pour diagnostiquer les problèmes.
- Mode inPrivate est maintenant pris en charge.
- Prise en charge améliorée pour les claviers non anglais.
- Correction d’un problème où l’info-bulles contenant des caractères non anglais présentait incorrectement.
- Problème de rendu de graphiques fixe qui Chrome utilisateurs affectés.
- Mise à jour de la redirection de fuseau horaire avec prise en charge complète de l’heure d’été.
- Amélioré le message d’erreur pour l’erreur de mémoire insuffisante.
- Divers correctifs de bogues.

## <a name="updates-for-version-100"></a>Mises à jour pour la version 1.0.0
*Date de publication : 07/16/2018*

- Client de bureau à distance par le web est désormais disponible.
- Les administrateurs peuvent globalement désactiver la télémétrie pour le client web.
- Divers correctifs de bogues.

## <a name="updates-for-version-090"></a>Mises à jour pour la version 0.9.0
*Date de publication : 07/05/2018*

- Expérience du client web de nouvelle connexion.
- Ne plus vous y êtes invité pour les informations d’identification lors du lancement d’une connexion de bureau ou application (authentification unique).
- Déplacer le client web vers une nouvelle URL : <https://server_FQDN/RDWeb/webclient/index.html>
- Redirection d’ajout fuseau horaire.
- Divers correctifs de bogues.

## <a name="updates-for-version-081"></a>Mises à jour pour la version 0.8.1
*Date de publication : 05/17/2018*

- Mises à jour pour traiter la mise à jour de CredSSP chiffrement oracle décrit dans CVE-2018-0886.
- Résolution des échecs de connexion pour certaines langues lors de l’impression est activée.
- Message d’erreur amélioré quand une passerelle ne fait pas partie du déploiement.
- **Aide** et **commentaires** options ont été ajoutées.

## <a name="updates-for-version-080"></a>Mises à jour pour la version 0.8.0
*Date de publication : 03/28/2018*

- Préversion publique initiale du client web.
- Copier/coller le texte dans le Presse-papiers avec **CTRL + C** et **CTRL + V**.
- Imprimer dans un fichier PDF.
- Dans 18 langues.
 

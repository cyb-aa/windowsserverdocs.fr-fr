---
title: Feuille de planification de la migration de MultiPoint services
description: Fournit des feuilles de calcul de planification pour vous aider à migrer vers MultiPoint services dans Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: d3d2ecca4062d28d210196d9191e08710eb731c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394630"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Feuille de planification de la migration de MultiPoint services

>S'applique à : Windows Server 2016

Utilisez les listes et les tables suivantes pour recueillir les paramètres dont vous avez besoin lors de la migration de MultiPoint services.

## <a name="source-server-settings"></a>Paramètres du serveur source

Vous trouverez les paramètres du serveur sous l’onglet dossier de **démarrage** dans le gestionnaire multipoint. Activez la case à cocher en regard de chaque paramètre en cours d’utilisation sur le serveur source.

- Autoriser un compte à avoir plusieurs sessions.
- Autoriser la gestion à distance de cet ordinateur.
- Autoriser l’analyse des postes de travail de cet ordinateur.
- Démarrez toujours en mode console.
- Ne pas afficher la notification de confidentialité à la première ouverture de session de l’utilisateur.
- Attribuez une adresse IP unique à chaque station.
- Autorisez la messagerie instantanée entre le tableau de bord MultiPoint et les sessions utilisateur sur cet ordinateur.
- Autorise l’orchestration des sessions utilisateur de l’administrateur et du tableau de bord MultiPoint.
- Autoriser les stations à utiliser le rendu matériel GPU.

## <a name="managed-servers-and-computers"></a>Serveurs gérés et ordinateurs

Notez les noms des ordinateurs et serveurs gérés. Vous pouvez trouver ces informations dans l’onglet dossier de **démarrage** du gestionnaire multipoint.

| Computer | Nom de l'ordinateur |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6\.        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>Stations

Enregistrez les stations locales et leurs paramètres. Vous pouvez trouver ces informations sous l’onglet **stations** dans le gestionnaire multipoint.

| #  | nom de la station | Compte d’utilisateur de connexion automatique | Orientation de l’affichage |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6\.  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>Administrateurs et utilisateurs du tableau de bord MultiPoint

Copiez les noms d’utilisateur pour les utilisateurs administrateurs et tableau de bord MultiPoint. Vous pouvez trouver ces informations dans l’onglet **utilisateurs** du gestionnaire multipoint.

Administrateurs :

- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :

Utilisateurs du tableau de bord :

- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :
- Nom d'utilisateur :

## <a name="vdi-template-and-virtual-desktops"></a>Modèle VDI et bureaux virtuels

Enregistrez les informations de modèle VDI et les noms des bureaux virtuels dans votre déploiement MultiPoint services. Vous pouvez trouver ces informations sous l’onglet **bureaux virtuels** dans le gestionnaire multipoint.

**Emplacement du modèle VDI**: 

| # | Nom du bureau virtuel      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |
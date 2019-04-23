---
title: Feuille de planification pour la migration de MultiPoint Services
description: Fournit la planification des feuilles de calcul pour vous aider à migrer vers les Services MultiPoint dans Windows Server 2016
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 864405bb-47ed-4c83-97a2-8df4c6e6f96b
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: a9d9b62bced9be90c658b79338c6f4ef07710fc3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880580"
---
# <a name="planning-worksheet-for-multipoint-services-migration"></a>Feuille de planification pour la migration de MultiPoint Services

>S'applique à : Windows Server 2016

Utilisez les listes et les tableaux suivants pour rassembler les paramètres que vous avez besoin pendant la migration de MultiPoint Services.

## <a name="source-server-settings"></a>Paramètres du serveur source

Vous pouvez trouver les paramètres du serveur sur le **accueil** onglet dans le gestionnaire MultiPoint. Cochez la case en regard de chaque paramètre en cours d’utilisation sur le serveur source.

- Autoriser plusieurs sessions sur un même compte.
- Ordinateur est autorisé à être gérés à distance.
- Autoriser la surveillance des postes de travail de cet ordinateur.
- Toujours démarrer en mode console.
- N’affiche pas de notification de confidentialité à la première ouverture de session utilisateur.
- Affecter une adresse IP unique à chaque station.
- Autoriser la messagerie instantanée entre le tableau de bord MultiPoint et les sessions utilisateur sur cet ordinateur.
- Autoriser l’orchestration d’administrateur et les sessions d’utilisateur de tableau de bord MultiPoint.
- Autoriser les stations à utiliser le rendu matériel GPU.

## <a name="managed-servers-and-computers"></a>Ordinateurs et serveurs gérés

Enregistrez les noms des serveurs gérés et des ordinateurs. Vous pouvez trouver ces informations sur le **accueil** onglet dans le gestionnaire MultiPoint.

| Computer | Nom de l'ordinateur |
|----------|---------------|
| 1        |               |
| 2        |               |
| 3        |               |
| 4        |               |
| 5        |               |
| 6        |               |
| 7        |               |
| 8        |               |
| 9        |               |
| 10       |               |


## <a name="stations"></a>Stations

Enregistrer des stations locales et leurs paramètres. Vous pouvez trouver ces informations sur le **Stations** onglet dans le gestionnaire MultiPoint.

| #  | nom de la station | Compte d’utilisateur d’ouverture de session automatique | Orientation de l’affichage |
|----|--------------|-------------------------|---------------------|
| 1  |              |                         |                     |
| 2  |              |                         |                     |
| 3  |              |                         |                     |
| 4  |              |                         |                     |
| 5  |              |                         |                     |
| 6  |              |                         |                     |
| 7  |              |                         |                     |
| 8  |              |                         |                     |
| 9  |              |                         |                     |
| 10 |              |                         |                     |

## <a name="administrators-and-multipoint-dashboard-users"></a>Les administrateurs et utilisateurs du tableau de bord MultiPoint

Copiez les noms d’utilisateur pour les administrateurs et les utilisateurs du tableau de bord MultiPoint. Vous pouvez trouver ces informations sur le **utilisateurs** onglet dans le gestionnaire MultiPoint.

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

## <a name="vdi-template-and-virtual-desktops"></a>Modèle d’infrastructure VDI et les bureaux virtuels

Enregistrer les informations de modèle d’infrastructure VDI et les noms des bureaux virtuels dans votre déploiement de MultiPoint Services. Vous pouvez trouver ces informations sur le **bureaux virtuels** onglet dans le gestionnaire MultiPoint.

**Emplacement du modèle d’infrastructure VDI**: 

| # | Nom du bureau virtuel      |
|---|---------------------------|
| 1 |                           |
| 2 |                           |
| 3 |                           |
| 4 |                           |
| 5 |                           |
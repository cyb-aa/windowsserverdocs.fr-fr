---
title: Suivi de vos licences d'accès client aux services Bureau à distance
description: Apprenez à effectuer le suivi des licences d'accès client de votre déploiement de services Bureau à distance.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: 7e5793427b4a294d90c7b9ebeb66bb27578be190
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80857332"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Suivi de vos licences d'accès client aux services Bureau à distance

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Vous pouvez utiliser l'outil Gestionnaire de licences des services Bureau à distance pour créer des rapports de suivi des licences d'accès client aux services Bureau à distance par utilisateur émises par un serveur de licences Bureau à distance.

> [!NOTE]
>  Si vous utilisez Azure AD Domain Services dans votre environnement, l'outil Gestionnaire de licences des services Bureau à distance ne vous permettra pas d'obtenir les licences d'accès client par utilisateur. Vous devrez effectuer le suivi des licences manuellement, soit par le biais d'événements de connexion, soit en interrogeant les connexions Bureau à distance actives via le service Broker pour les connexions, soit par l'intermédiaire d'un autre mécanisme qui fonctionne pour vous. 

Procédez comme suit pour générer un rapport de licences d'accès client par utilisateur :

1. Dans le Gestionnaire de licences des services Bureau à distance, cliquez avec le bouton droit sur le serveur de licences, cliquez sur **Créer un rapport**, puis cliquez sur **Utilisation de licences d'accès client par utilisateur**.
2. Définissez l'étendue du rapport - sélectionnez l'une des options suivantes :
   - Domaine complet : domaine dont le serveur de licences est membre.
   - Unité d'organisation : toute unité d'organisation du domaine dont le serveur de licences est membre.
   - Domaine entier et tous les domaines approuvés : peut inclure des domaines d'autres forêts. La sélection de cette option peut rallonger le temps nécessaire à la création du rapport.

   La sélection que vous effectuez détermine dans quels comptes d'utilisateur AD DS les informations des licences d'accès client aux services Bureau à distance par utilisateur sont recherchées pour générer le rapport.
3. Cliquez sur **Créer un rapport**. Le rapport est créé et un message apparaît pour confirmer la création. Cliquez sur **OK** pour fermer le message.

Le rapport que vous avez créé apparaît dans la section Rapports sous le nœud du serveur de licences. Le rapport fournit les informations suivantes :

- Date et heure de création du rapport
- Étendue du rapport (par exemple, Domaine, Unité d'organisation = Ventes, ou Tous les domaines approuvés)
- Nombre de licences d'accès client aux services Bureau à distance par utilisateur installées sur le serveur de licences
- Nombre de licences d'accès client aux services Bureau à distance par utilisateur qui ont été émises par le serveur de licences spécifique et correspondant à l'étendue du rapport

Vous pouvez également enregistrer le rapport sous forme de fichier CSV dans un dossier de l'ordinateur. Pour enregistrer le rapport, cliquez avec le bouton droit sur le rapport que vous souhaitez enregistrer, cliquez sur Enregistrer sous, puis spécifiez le nom du fichier et l'emplacement d'enregistrement du rapport.

Les rapports que vous créez sont répertoriés dans le nœud Rapports, sous le nœud du serveur de licences du Gestionnaire de licences des services Bureau à distance. Si vous n'avez plus besoin d'un rapport, vous pouvez le supprimer.

---
title: Effectuer le suivi de vos licences d’accès client des Services Bureau à distance (RDS CAL)
description: Découvrez comment effectuer le suivi des licences d’accès client dans votre déploiement de services Bureau à distance.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80d82d30-3ad0-4a8c-9a9b-2773c47eee19
author: lizap
ms.author: elizapo
ms.date: 05/11/2017
manager: dongill
ms.openlocfilehash: ed7490b2b61eb516d43b280b67fcefaeedb3e225
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59890620"
---
# <a name="track-your-remote-desktop-services-client-access-licenses-rds-cals"></a>Effectuer le suivi de vos licences d’accès client des Services Bureau à distance (RDS CAL)

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser l’outil Gestionnaire de licences bureau à distance pour créer des rapports pour effectuer le suivi de la RDS CAL par utilisateur qui ont été émis par un serveur de licences bureau à distance.

> [!NOTE]
>  Si vous utilisez Azure AD Domain Services dans votre environnement, l’outil Gestionnaire de licences bureau à distance ne fonctionnera pas pour obtenir des licences d’accès client par utilisateur. Au lieu de cela, vous devez le suivi des licences manuellement, soit via les événements d’ouverture de session, les connexions Bureau à distance actives d’interrogation via le répartiteur de connexion ou un autre mécanisme qui vous convient. 

Utilisez les étapes suivantes pour générer un rapport de licences d’accès client utilisateur par :

1. Dans le Gestionnaire de licences bureau à distance sur le serveur de licences, cliquez **créer un rapport**, puis cliquez sur **par l’utilisation de licences d’accès client utilisateur**.
2. Définir l’étendue du rapport - Sélectionnez une des opérations suivantes :
   - Intégralité du domaine - le domaine dans lequel le serveur de licences est membre.
   - Unité d’organisation - toute unité d’organisation au sein du domaine dans lequel le serveur de licences est membre.
   - Domaine entier et tous les domaines approuvés - peuvent inclure des domaines dans d’autres forêts. Cette option peut augmenter le temps nécessaire pour créer le rapport.

   La sélection que vous effectuez détermine quels comptes d’utilisateur dans AD DS sont recherchés pour des informations de licence des services Bureau à distance par utilisateur générer le rapport.
3. Cliquez sur **créer rapport**. Le rapport est créé et un message s’affiche pour confirmer que le rapport a été créé. Cliquez sur **OK** pour fermer le message.

Le rapport que vous avez créé apparaît dans la section rapports sous le nœud pour le serveur de licences. Le rapport fournit les informations suivantes :

- Date et heure de création du rapport
- L’étendue du rapport (par exemple, domaine, unité d’organisation = Sales, ou tous les domaines approuvés)
- Le nombre de services Bureau à distance des CAL par utilisateur qui sont installés sur le serveur de licences
- Le nombre de services Bureau à distance des CAL par utilisateur qui ont été émis par le serveur de licences spécifique à l’étendue du rapport

Vous pouvez également enregistrer le rapport dans un fichier CSV vers un emplacement de dossier sur l’ordinateur. Pour enregistrer le rapport, cliquez sur le rapport que vous souhaitez enregistrer et cliquez sur Enregistrer sous, puis spécifiez le nom de fichier et un emplacement pour enregistrer le rapport.

Les rapports que vous créez sont répertoriés dans le nœud rapports sous le nœud du serveur de licences dans le Gestionnaire de licences bureau à distance. Si vous n’avez plus besoin un rapport, vous pouvez le supprimer.

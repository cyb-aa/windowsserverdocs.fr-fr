---
title: Réseau de collecte de données de serveur de stratégie utilisateur
description: Quelles informations sont utilisées pour aider à authentifier les utilisateurs par le serveur de stratégie réseau dans Windows Server 2016.
author: MicrosoftGuyJFlo
manager: mtillman
ms.author: joflore
ms.reviewer: richagi
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: ''
ms.date: 05/01/2018
ms.openlocfilehash: 5bddd22c9c2f954435cc6ce37347d18c76ee7de3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888740"
---
# <a name="network-policy-server-user-data-collection"></a>Réseau de collecte de données de serveur de stratégie utilisateur

Ce document explique comment trouver les informations utilisateur collectées par le serveur NPS (Network Policy) dans l’événement vous souhaitez supprimer.

>[!Note]
>Si vous êtes intéressé de l’affichage ou la suppression des données personnelles, passez en revue les conseils de Microsoft dans le [requêtes Windows pour le RGPD](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) site. Si vous recherchez des informations générales sur RGPD, consultez le [section RGPD du portail de confiance du Service](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informations collectées par le serveur NPS

- Horodateur
- Horodateur de l’événement
- Nom d'utilisateur
- Nom d’utilisateur qualifié complet
- Adresse IP du client
- Fournisseur du client
- Nom convivial du client
- Type d’authentification
- Nombreux autres champs concernant le protocole RADIUS

## <a name="gather-data-from-nps"></a>Collecter les données de serveur NPS

Si les données de gestion sont activées et configurées, les enregistrements de tentatives d’authentification NPS d’un utilisateur peuvent être obtenus à partir de SQL Server ou les fichiers journaux en fonction de la configuration. 

Si les données de gestion sont configurées pour SQL Server, la requête pour tous les enregistrements où nom_utilisateur = `'<username>'`.

Si les données de gestion sont configurées pour un fichier journal, puis recherchez le fichier journal pour le `<username>` pour rechercher toutes les entrées de journal.

Stratégie de réseau et les Services d’accès aux entrées de journal des événements sont considérés comme pyramidaux pour les données de gestion et ne doivent être collectées.

Si les données de gestion ne sont pas activées, les enregistrements de tentatives d’authentification NPS d’un utilisateur peuvent être obtenus à partir du journal des événements de stratégie réseau et Services d’accès en recherchant le `<username>`.

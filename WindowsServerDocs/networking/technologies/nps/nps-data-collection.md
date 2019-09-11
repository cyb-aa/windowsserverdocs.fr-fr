---
title: Collecte des données utilisateur du serveur de stratégie réseau
description: Les informations utilisées pour aider à authentifier les utilisateurs par le serveur de stratégie réseau dans Windows Server 2016.
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
ms.openlocfilehash: cd145402ed70aa52da7188dee9dd64ce17fea155
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871878"
---
# <a name="network-policy-server-user-data-collection"></a>Collecte des données utilisateur du serveur de stratégie réseau

Ce document explique comment rechercher des informations utilisateur collectées par le serveur NPS (Network Policy Server) dans l’événement que vous souhaitez supprimer.

>[!Note]
>Si vous souhaitez afficher ou supprimer des données personnelles, consultez les conseils de Microsoft relatifs aux demandes de l' [objet des données Windows pour le site RGPD](https://docs.microsoft.com/microsoft-365/compliance/gdpr-dsr-windows) . Si vous recherchez des informations générales sur RGPD, reportez-vous à la [section RGPD du portail Service Trust](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted).

## <a name="information-collected-by-nps"></a>Informations collectées par le serveur NPS

- Timestamp
- Horodateur de l’événement
- Nom d’utilisateur
- Nom d’utilisateur complet
- Adresse IP du client
- Fournisseur du client
- Nom convivial du client
- Type d’authentification
- Nombreux autres champs relatifs au protocole RADIUS

## <a name="gather-data-from-nps"></a>Collecter des données à partir du serveur NPS

Si les données de gestion sont activées et configurées, les enregistrements des tentatives d’authentification NPS d’un utilisateur peuvent être obtenus à partir de SQL Server ou des fichiers journaux en fonction de la configuration. 

Si les données de gestion des comptes sont configurées pour SQL Server, recherchez tous `'<username>'`les enregistrements où user_name =.

Si les données de gestion des comptes sont configurées pour un fichier journal, recherchez toutes `<username>` les entrées de journal dans le fichier journal.

Les entrées du journal des événements des services de stratégie et d’accès réseau sont considérées comme dupliquées dans les données de comptabilité et n’ont pas besoin d’être collectées.

Si les données de gestion ne sont pas activées, les enregistrements des tentatives d’authentification NPS d’un utilisateur peuvent être obtenus à partir du journal des événements des services `<username>`de stratégie et d’accès réseau en recherchant le.

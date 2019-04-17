---
title: "Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers (FSRM)"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 7/7/2017
description: "Le Gestionnaire de ressources du serveur de fichiers (FSRM) est un outil qui vous permet de gérer et de classer des données sur un serveur de fichiers Windows Server."
ms.openlocfilehash: ddfc0a0f4bede89a3c3a624d4f128717d0f1ac08
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers

> S’applique à: WindowsServer (canal semi-annuel), WindowsServer2016, WindowsServer2012R2, WindowsServer2012, WindowsServer2008R2

Le Gestionnaire de ressources du serveur de fichiers (FSRM) est un service de rôle de Windows Server qui permet de gérer et de classer les données stockées sur des serveurs de fichiers. Le Gestionnaire de ressources du serveur de fichiers comprend les fonctionnalités suivantes:
  
-   L’**infrastructure de classification des fichiers** aide à mieux appréhender vos données en automatisant des processus de classification qui vous permettront de les gérer avec plus d’efficacité. Vous pouvez classer les fichiers et appliquer des stratégies en fonction de cette classification. Parmi les exemples de stratégie, citons le contrôle d’accès dynamique pour restreindre l’accès aux fichiers, le chiffrement des fichiers et l’expiration des fichiers. Vous pouvez soit classer les fichiers automatiquement en utilisant des règles de classification de fichiers, soit les classer manuellement en modifiant les propriétés d’un fichier ou d’un dossier sélectionné.  
  
-   Les**tâches de gestion de fichiers** vous permettent d’appliquer une stratégie ou action conditionnelle à des fichiers en fonction de leur classification. Les conditions d’une tâche de gestion de fichiers incluent l’emplacement du fichier, les propriétés de classification, la date de création du fichier, la date de la dernière modification du fichier ou le dernier accès au fichier. Dans le cadre d’une tâche de gestion de fichiers, vous pouvez faire expirer des fichiers, les chiffrer ou exécuter une commande personnalisée.  
  
-   La**gestion de quota** permet de limiter l’espace autorisé pour un volume ou un dossier, et des quotas peuvent être automatiquement appliqués aux nouveaux dossiers créés sur un volume. Vous pouvez également définir des modèles de quota qui peuvent être appliqués aux nouveaux volumes ou dossiers.  
  
-   La**gestion des filtres de fichiers** permet de contrôler les types de fichiers qu’un utilisateur peut stocker sur un serveur de fichiers. Vous pouvez limiter les extensions pouvant être stockées sur vos fichiers partagés. Par exemple, vous pouvez créer un filtre de fichiers qui interdit le stockage de fichiers dotés d’une extension MP3 dans les dossiers partagés personnels sur un serveur de fichiers.  
  
-   Les**rapports de stockage** permettent d’identifier des tendances au niveau de l’utilisation des disques, ainsi que le mode de classification des données. Vous pouvez également analyser un groupe sélectionné d’utilisateurs afin de détecter toute tentative d’enregistrement de fichiers non autorisés.  
  
 Les fonctionnalités fournies avec le Gestionnaire de ressources du serveur de fichiers peuvent être configurées et gérées à l’aide de la console MMC (Microsoft Management Console) Gestionnaire de ressources du serveur de fichiers ou à l’aide de Windows PowerShell.  
  
> [!IMPORTANT]
>  Le Gestionnaire de ressources du serveur de fichiers prend uniquement en charge les volumes formatés avec le système de fichiers NTFS. Le système de fichiers résilient n’est pas pris en charge.  
  
## <a name="practical-applications"></a>Applications pratiques  
 Parmi les cas pratiques du Gestionnaire de ressources du serveur de fichiers, citons les suivants:  
  
-   Vous pouvez utiliser l’infrastructure de classification des fichiers avec le scénario de contrôle d’accès dynamique pour créer une stratégie qui accorde l’accès à des fichiers et à des dossiers en fonction de la façon dont les fichiers sont classifiés sur le serveur de fichiers.  
  
-   Vous pouvez créer une règle de classification de fichiers qui identifie tout fichier contenant au moins 10 numéros de sécurité sociale comme possédant des informations d’identification personnelle.  
  
-   Vous pouvez faire expirer les fichiers qui n’ont pas été modifiés au cours des 10dernières années.  
  
-   Vous pouvez créer un quota de 200 mégaoctets pour le répertoire de base de chaque utilisateur et les notifier lorsqu’ils utilisent 180 mégaoctets.  
  
-   Vous pouvez interdire le stockage des fichiers musicaux dans les dossiers partagés personnels.  
  
-   Vous pouvez créer un rapport qui s’exécute tous les dimanches soirs à minuit et qui génère une liste des fichiers les plus récemment consultés au cours des deux derniers jours. Vous pouvez ainsi déterminer l’activité de stockage du week-end et planifier en conséquence le temps d'arrêt du serveur.  

## <a name="see-also"></a>Articles associés

- [Nouveautés du Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/dn383587.aspx)
- [Contrôle d'accès dynamique](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 
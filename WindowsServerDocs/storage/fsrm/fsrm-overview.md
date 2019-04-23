---
title: Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: File Server Resource Manager (FSRM) est un outil qui vous permet de gérer et classer les données sur un serveur de fichiers Windows Server.
ms.openlocfilehash: 107d08f247fc56720ccc3d11a3db88c77377257c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870720"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers

> S’applique à : Windows Server (canal semi-annuel), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le Gestionnaire de ressources du serveur de fichiers (FSRM) est un service de rôle de Windows Server qui permet de gérer et de classer les données stockées sur des serveurs de fichiers. Vous pouvez utiliser File Server Resource Manager pour classifier les fichiers, effectuer des tâches en fonction de ces classifications, définir des quotas sur les dossiers et créer des rapports de surveillance de l’utilisation de stockage automatiquement.

C’est un point de petite, mais nous avons également [ajouté la possibilité de désactiver les journaux de modification](#whats-new) dans Windows Server, version 1803.

## <a name="features"></a>Fonctionnalités

Le Gestionnaire de ressources du serveur de fichiers comprend les fonctionnalités suivantes :

-   [Gestion de quota](quota-management.md) vous permet de limiter l’espace autorisé pour un volume ou un dossier, et ils peuvent être automatiquement appliqués aux nouveaux dossiers créés sur un volume. Vous pouvez également définir des modèles de quota qui peuvent être appliqués aux nouveaux volumes ou dossiers.  
-   [Infrastructure de Classification des fichiers](classification-management.md) fournit un aperçu de vos données en automatisant les processus de classification afin que vous pouvez gérer plus efficacement vos données. Vous pouvez classer les fichiers et appliquer des stratégies en fonction de cette classification. Parmi les exemples de stratégie, citons le contrôle d’accès dynamique pour restreindre l’accès aux fichiers, le chiffrement des fichiers et l’expiration des fichiers. Vous pouvez soit classer les fichiers automatiquement en utilisant des règles de classification de fichiers, soit les classer manuellement en modifiant les propriétés d’un fichier ou d’un dossier sélectionné.
-   [Tâches de gestion de fichiers](file-management-tasks.md) vous permet d’appliquer une stratégie ou action conditionnelle à des fichiers en fonction de leur classification. Les conditions d’une tâche de gestion de fichiers incluent l’emplacement du fichier, les propriétés de classification, la date de création du fichier, la date de la dernière modification du fichier ou le dernier accès au fichier. Dans le cadre d’une tâche de gestion de fichiers, vous pouvez faire expirer des fichiers, les chiffrer ou exécuter une commande personnalisée.
-   [Gestion des filtres](file-screening-management.md) vous permet de contrôler les types de fichiers qu’un utilisateur peut stocker sur un serveur de fichiers. Vous pouvez limiter les extensions pouvant être stockées sur vos fichiers partagés. Par exemple, vous pouvez créer un filtre de fichiers qui interdit le stockage de fichiers dotés d’une extension MP3 dans les dossiers partagés personnels sur un serveur de fichiers.
-   [Rapports de stockage](storage-reports-management.md) vous aider à identifier les tendances dans l’utilisation du disque et comment vos données sont classées. Vous pouvez également analyser un groupe sélectionné d’utilisateurs afin de détecter toute tentative d’enregistrement de fichiers non autorisés.  
  
Les fonctionnalités incluses avec le Gestionnaire de ressources du serveur de fichiers peuvent être configurées et gérées à l’aide de l’application de File Server Resource Manager ou à l’aide de Windows PowerShell.
  
> [!IMPORTANT]
>  Le Gestionnaire de ressources du serveur de fichiers prend uniquement en charge les volumes formatés avec le système de fichiers NTFS. Le système de fichiers résilient n’est pas pris en charge.  
  
## <a name="practical-applications"></a>Cas pratiques  
 Parmi les cas pratiques du Gestionnaire de ressources du serveur de fichiers, citons les suivantes :  
  
-   Vous pouvez utiliser l’infrastructure de classification des fichiers avec le scénario de contrôle d’accès dynamique pour créer une stratégie qui accorde l’accès à des fichiers et à des dossiers en fonction de la façon dont les fichiers sont classifiés sur le serveur de fichiers.  
  
-   Vous pouvez créer une règle de classification de fichiers qui identifie tout fichier contenant au moins 10 numéros de sécurité sociale comme possédant des informations d’identification personnelle.  
  
-   Vous pouvez faire expirer les fichiers qui n’ont pas été modifiés au cours des 10 dernières années.  
  
-   Vous pouvez créer un quota de 200 mégaoctets pour le répertoire de base de chaque utilisateur et les notifier lorsqu’ils utilisent 180 mégaoctets.  
  
-   Vous pouvez interdire le stockage des fichiers musicaux dans les dossiers partagés personnels.  
  
-   Vous pouvez créer un rapport qui s’exécute tous les dimanches soirs à minuit et qui génère une liste des fichiers les plus récemment consultés au cours des deux derniers jours. Vous pouvez ainsi déterminer l’activité de stockage du week-end et planifier en conséquence le temps mort du serveur.  

## <a name="whats-new"></a>Quelles sont les nouveautés - FSRM empêche de créer des journaux de modification

En commençant par Windows Server, version 1803, vous pouvez maintenant empêcher le service de File Server Resource Manager à partir de la création d’un journal des modifications (également appelé un journal USN) sur les volumes au démarrage du service. Cela permet d’économiser un peu d’espace sur chaque volume, mais désactivera la classification des fichiers en temps réel.

Pour les anciennes nouvelles fonctionnalités, consultez [quelles sont les nouveautés dans le Gestionnaire de ressources du serveur de fichiers](https://technet.microsoft.com/library/dn383587.aspx).

Pour éviter les File Server Resource Manager à partir de la création d’un journal des modifications sur certains ou tous les volumes au démarrage du service, utilisez les étapes suivantes : 

1. Arrêtez le service SRMSVC. Par exemple, ouvrez une session PowerShell en tant qu’administrateur, puis entrez `Stop-Service SrmSvc`.
2. Supprimer le journal USN pour les volumes que vous souhaitez économiser l’espace à l’aide de la commande fsutil : 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Par exemple : `fsutil usn deletejournal /d c:`

3. Ouvrez l’Éditeur du Registre, par exemple, en tapant `regedit` dans la même session PowerShell.
4. Accédez à la clé suivante : **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Si vous le souhaitez à ignorer modifier la création du journal pour l’ensemble du serveur (ignorez cette étape si vous souhaitez désactiver uniquement sur des volumes spécifiques) :
    1. Cliquez sur le **paramètres** clé, puis sélectionnez **New** > **valeur DWORD (32 bits)**. 
    1. Nommez la valeur `SkipUSNCreationForSystem`.
    1. Définissez la valeur sur **1** (en hexadécimal).
6. Si vous le souhaitez ignorer la création du journal de modification pour des volumes spécifiques :
    1. Obtenir les chemins d’accès de volume que vous souhaitez ignorer à l’aide de la `fsutil volume list` commande ou la commande PowerShell suivante :
        ```PowerShell
        Get-Volume | Format-Table DriveLetter,FileSystemLabel,Path
        ```
       Voici un exemple de sortie :

       ```
        DriveLetter FileSystemLabel Path
        ----------- --------------- ----
                    System Reserved \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        C                           \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
       ```
    2. Revenez dans Éditeur du Registre, cliquez sur le **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** clé, puis sélectionnez **New** > **chaînes multiples Valeur**.
    3. Nommez la valeur `SkipUSNCreationForVolumes`.
    4. Entrez le chemin d’accès de chaque volume sur lequel vous ignorez la création d’un journal des modifications, placer chaque chemin d’accès sur une ligne distincte. Exemple :

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > Éditeur du Registre peut vous dire qu’il supprimé des chaînes vides, affichage de cet avertissement, vous pouvez ignorer en toute sécurité : *Données de type REG_MULTI_SZ ne peut pas contenir des chaînes vides. Éditeur du Registre supprimera toutes les chaînes vides trouvées.*

7. Démarrez le service SRMSVC. Par exemple, dans une session PowerShell, entrez `Start-Service SrmSvc`.



## <a name="see-also"></a>Voir aussi

- [Contrôle d’accès dynamique](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 
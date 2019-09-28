---
title: Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers
ms.prod: windows-server
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 5/14/2018
description: Le Gestionnaire des ressources de serveur de fichiers (FSRM) est un outil qui vous permet de gérer et de classer des données sur un serveur de fichiers Windows Server.
ms.openlocfilehash: 719176307afc320ad676fd1acfc07ad9d15920cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394167"
---
# <a name="file-server-resource-manager-fsrm-overview"></a>Vue d’ensemble du Gestionnaire de ressources du serveur de fichiers

> S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canal semi-annuel), 

Le Gestionnaire de ressources du serveur de fichiers (FSRM) est un service de rôle de Windows Server qui permet de gérer et de classer les données stockées sur des serveurs de fichiers. Vous pouvez utiliser des Gestionnaire des ressources de serveur de fichiers pour classifier automatiquement des fichiers, effectuer des tâches en fonction de ces classifications, définir des quotas sur les dossiers et créer des rapports d’analyse de l’utilisation du stockage.

C’est un petit point, mais nous avons également [ajouté la possibilité de désactiver les journaux de modification](#whats-new) dans Windows Server, version 1803.

## <a name="features"></a>Fonctionnalités

Le Gestionnaire de ressources du serveur de fichiers comprend les fonctionnalités suivantes :

-   La [gestion des quotas](quota-management.md) vous permet de limiter l’espace autorisé pour un volume ou un dossier, et elles peuvent être appliquées automatiquement aux nouveaux dossiers créés sur un volume. Vous pouvez également définir des modèles de quota qui peuvent être appliqués aux nouveaux volumes ou dossiers.  
-   [Infrastructure de classification des fichiers](classification-management.md) fournit des informations sur vos données en automatisant les processus de classification afin que vous puissiez gérer vos données plus efficacement. Vous pouvez classer les fichiers et appliquer des stratégies en fonction de cette classification. Parmi les exemples de stratégie, citons le contrôle d’accès dynamique pour restreindre l’accès aux fichiers, le chiffrement des fichiers et l’expiration des fichiers. Vous pouvez soit classer les fichiers automatiquement en utilisant des règles de classification de fichiers, soit les classer manuellement en modifiant les propriétés d’un fichier ou d’un dossier sélectionné.
-   Les [tâches de gestion de fichiers](file-management-tasks.md) vous permettent d’appliquer une stratégie ou une action conditionnelle à des fichiers en fonction de leur classification. Les conditions d’une tâche de gestion de fichiers incluent l’emplacement du fichier, les propriétés de classification, la date de création du fichier, la date de la dernière modification du fichier ou le dernier accès au fichier. Dans le cadre d’une tâche de gestion de fichiers, vous pouvez faire expirer des fichiers, les chiffrer ou exécuter une commande personnalisée.
-   La [gestion du filtrage de fichiers](file-screening-management.md) vous permet de contrôler les types de fichiers que l’utilisateur peut stocker sur un serveur de fichiers. Vous pouvez limiter les extensions pouvant être stockées sur vos fichiers partagés. Par exemple, vous pouvez créer un filtre de fichiers qui interdit le stockage de fichiers dotés d’une extension MP3 dans les dossiers partagés personnels sur un serveur de fichiers.
-   Les [rapports de stockage](storage-reports-management.md) vous aident à identifier les tendances d’utilisation des disques et la façon dont vos données sont classées. Vous pouvez également analyser un groupe sélectionné d’utilisateurs afin de détecter toute tentative d’enregistrement de fichiers non autorisés.  
  
Les fonctionnalités incluses dans les Gestionnaire des ressources du serveur de fichiers peuvent être configurées et gérées à l’aide de l’application Gestionnaire des ressources serveur de fichiers ou à l’aide de Windows PowerShell.
  
> [!IMPORTANT]
>  Le Gestionnaire de ressources du serveur de fichiers prend uniquement en charge les volumes formatés avec le système de fichiers NTFS. Le système de fichiers résilient n’est pas pris en charge.  
  
## <a name="practical-applications"></a>Cas pratiques  
 Parmi les cas pratiques du Gestionnaire de ressources du serveur de fichiers, citons les suivantes :  
  
-   Vous pouvez utiliser l’infrastructure de classification des fichiers avec le scénario de contrôle d’accès dynamique pour créer une stratégie qui accorde l’accès à des fichiers et à des dossiers en fonction de la façon dont les fichiers sont classifiés sur le serveur de fichiers.  
  
-   Vous pouvez créer une règle de classification de fichiers qui identifie tout fichier contenant au moins 10 numéros de sécurité sociale comme possédant des informations d’identification personnelle.  
  
-   Vous pouvez faire expirer les fichiers qui n’ont pas été modifiés au cours des 10 dernières années.  
  
-   Créez un quota de 200 mégaoctets pour le répertoire de démarrage de chaque utilisateur et avertissez-le lorsqu’il utilise 180 mégaoctets.  
  
-   Vous pouvez interdire le stockage des fichiers musicaux dans les dossiers partagés personnels.  
  
-   Vous pouvez créer un rapport qui s’exécute tous les dimanches soirs à minuit et qui génère une liste des fichiers les plus récemment consultés au cours des deux derniers jours. Vous pouvez ainsi déterminer l’activité de stockage du week-end et planifier en conséquence le temps mort du serveur.  

## <a name="whats-new"></a>Nouveautés-empêcher FSRM de créer des journaux de modifications

À compter de Windows Server, version 1803, vous pouvez désormais empêcher le service de Gestionnaire des ressources de serveur de fichiers de créer un journal des modifications (également appelé journal USN) sur les volumes au démarrage du service. Cela peut économiser un peu d’espace sur chaque volume, mais la classification des fichiers en temps réel est désactivée.

Pour les nouvelles fonctionnalités plus anciennes, consultez [Nouveautés du serveur de fichiers gestionnaire des ressources](https://technet.microsoft.com/library/dn383587.aspx).

Pour empêcher les Gestionnaire des ressources du serveur de fichiers de créer un journal des modifications sur tout ou partie des volumes au démarrage du service, procédez comme suit : 

1. Arrêtez le service SRMSVC. Par exemple, ouvrez une session PowerShell en tant qu’administrateur et `Stop-Service SrmSvc`entrez.
2. Supprimez le journal USN pour les volumes pour lesquels vous souhaitez économiser de l’espace à l’aide de la commande fsutil : 

      ```
      fsutil usn deletejournal /d <VolumeName>
      ```
    Par exemple : `fsutil usn deletejournal /d c:`

3. Ouvrez l’éditeur du Registre, par exemple, `regedit` en tapant dans la même session PowerShell.
4. Accédez à la clé suivante : **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings**
5. Pour ignorer éventuellement la modification de la création du journal pour l’ensemble du serveur (ignorez cette étape si vous souhaitez la désactiver uniquement sur des volumes spécifiques) :
    1. Cliquez avec le bouton droit sur la clé des **paramètres** , puis sélectionnez **nouvelle** > **valeur DWORD (32 bits)** . 
    1. Nommez la `SkipUSNCreationForSystem`valeur.
    1. Définissez la valeur sur **1** (hexadécimal).
6. Pour éventuellement ignorer la création du journal des modifications pour des volumes spécifiques :
    1. Récupérez les chemins de volume que vous souhaitez ignorer à `fsutil volume list` l’aide de la commande ou de la commande PowerShell suivante :
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
    2. De retour dans l’éditeur du Registre, cliquez avec le bouton droit sur la clé **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\SrmSvc\Settings** , puis sélectionnez **nouvelle** > **valeur de chaînes multiples**.
    3. Nommez la `SkipUSNCreationForVolumes`valeur.
    4. Entrez le chemin d’accès de chaque volume sur lequel vous ignorez la création d’un journal des modifications, en plaçant chaque chemin sur une ligne distincte. Exemple :

        ```
        \\?\Volume{8d3c9e8a-0000-0000-0000-100000000000}\
        \\?\Volume{8d3c9e8a-0000-0000-0000-501f00000000}\
        ```

        > [!NOTE] 
        > L’éditeur du Registre peut vous indiquer qu’il a supprimé des chaînes vides, en affichant cet avertissement que vous pouvez ignorer en toute sécurité : *Data de type REG_MULTI_SZ ne peut pas contenir de chaînes vides. L’éditeur du Registre va supprimer toutes les chaînes vides trouvées.*

7. Démarrez le service SRMSVC. Par exemple, dans une session PowerShell, `Start-Service SrmSvc`entrez.



## <a name="see-also"></a>Voir aussi

- [Access Control dynamique](https://technet.microsoft.com/library/dn408191(v=ws.11).aspx) 
---
title: Déployer des espaces de stockage sur un serveur autonome
description: Décrit comment déployer des espaces de stockage sur un serveur autonome basée sur Windows Server 2012.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 40265e767ac9aca05386c0893def259aca3a5633
ms.sourcegitcommit: e73fbe1046a8bd2bf4f24ccffc11465ad8dfab1d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/07/2019
ms.locfileid: "8992532"
---
# Déployer des espaces de stockage sur un serveur autonome

>S’applique à: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment déployer des espaces de stockage sur un serveur autonome. Pour plus d’informations sur la création d’un espace de stockage en cluster, consultez [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Pour créer un espace de stockage, vous devez d’abord créer un ou plusieurs pools de stockage. Un pool de stockage est un ensemble de disques physiques. Un pool de stockage permet d’agrégation de stockage, extension de la capacité élastique et d’administrateur délégués.

À partir d’un pool de stockage, vous pouvez créer un ou plusieurs disques virtuels. Ces disques virtuels sont également appelés *espaces de stockage*. Un espace de stockage s’affiche au système d’exploitation Windows comme un disque standard à partir duquel vous pouvez créer des volumes formatés. Lorsque vous créez un disque virtuel par le biais de l’interface utilisateur de Services de fichiers et stockage, vous pouvez configurer le type de résilience (simple, mise en miroir ou parité), le type d’approvisionnement (mince ou fixe) et la taille. Par le biais de Windows PowerShell, vous pouvez définir des paramètres supplémentaires telles que le nombre de colonnes, la valeur d’entrelacement et quel physique de disques dans le pool à utiliser. Pour plus d’informations sur ces paramètres supplémentaires, voir [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) et [ce que sont des colonnes et la façon dont les espaces de stockage décidez combien d'entre eux devront utiliser?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) de stockage espaces Forum aux Questions (FAQ).

>[!NOTE]
>Vous ne pouvez pas utiliser un espace de stockage pour héberger le système d’exploitation Windows.

À partir d’un disque virtuel, vous pouvez créer un ou plusieurs volumes. Lorsque vous créez un volume, vous pouvez configurer la taille de la lettre de lecteur ou dossier, système de fichiers (système de fichiers NTFS ou le système de fichiers résilient (ReFS)), taille d’unité d’allocation et un nom de volume facultatif.

L’exemple suivant illustre le flux de travail d’espaces de stockage.

![Workflow d’espaces de stockage](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figure 1: Flux de travail les espaces de stockage**

>[!NOTE]
>Cette rubrique contient des applets de commande exemple Windows PowerShell que vous pouvez utiliser pour automatiser une partie des procédures décrites. Pour plus d’informations, voir [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## Conditions préalables

Pour utiliser des espaces de stockage sur un serveur de 2012−based autonome Windows Server, assurez-vous que les disques physiques que vous voulez utiliser respecter la configuration suivante.

> [!IMPORTANT]
> Si vous souhaitez apprendre à déployer des espaces de stockage sur un cluster de basculement, consultez [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Un déploiement de cluster de basculement a différentes conditions requises, telles que les types de bus de disque pris en charge, types de résilience pris en charge et le nombre minimal requis de disques.

|Domaine|Exigence|Notes|
|---|---|---|
|Types de bus de disque|-Serial Attached SCSI (SAS)<br>-Série avancées technologiques pièce jointe (SATA)<br>-iSCSI et contrôleurs Fibre Channel. |Vous pouvez également utiliser des lecteurs USB. Toutefois, il n’est pas optimal pour utiliser les lecteurs USB dans un environnement de serveur.<br>Les espaces de stockage est pris en charge sur iSCSI et les contrôleurs de Fibre Channel (FC) tant que les disques virtuels créés par-dessus eux sont non résilient (Simple avec n’importe quel nombre de colonnes).<br>|
|Configuration de disque|-Les disques physiques doivent être au moins 4 Go<br>-Disques doivent être vides et non formaté. Ne créez pas de volumes.||
|Considérations relatives à la HBA|-Adaptateurs de bus hôte simple (HBA) qui ne prennent pas en charge les fonctionnalités RAID sont recommandées.<br>-Si RAID prenant en charge, HBA doit être en mode non RAID avec toutes les fonctionnalités RAID désactivée<br>-Cartes ne doivent pas abstraite les disques physiques, les données du cache, ou masquent tous les appareils joints. Cela inclut les services de boîtier qui sont fournis par les périphériques (JBOD) juste-a-série-de-disques connectés. |Les espaces de stockage est compatible uniquement avec HBA où vous pouvez désactiver complètement toutes les fonctionnalités RAID.|
|Boîtiers JBOD|-Les boîtiers JBOD sont facultatives<br>-Recommandé d’utiliser des espaces de stockage certifiés boîtiers répertoriés dans le catalogue Windows Server<br>-Si vous utilisez un boîtier JBOD, vérifiez auprès de votre fournisseur de stockage que le boîtier prend en charge les espaces de stockage pour vous assurer que toutes les fonctionnalités<br>-Pour déterminer si le boîtier JBOD prend en charge l’identification boîtier et emplacement, exécutez l’applet de commande Windows PowerShell suivante:<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si les champs **EnclosureNumber** et **SlotNumber** contiennent des valeurs, le boîtier prend en charge ces fonctionnalités.||

Pour planifier le nombre de disques physiques et le type de résilience souhaité pour un déploiement de serveur autonome, suivez les instructions ci-dessous.

|Type de résilience|Configuration requise de disque|Quand l’utiliser|
|---|---|---|
|**Simplicité**<br><br>-Répartit les données sur les disques physiques<br>-Optimise la capacité du disque et augmente le débit<br>-Aucune résilience (ne protège pas contre la défaillance du disque)<br><br><br><br><br><br><br>|Nécessite au moins un disque physique.|N’utilisez pas pour héberger des données irremplaçables. Espaces simples protège pas contre les défaillances de disque.<br><br>Utilisez cette option pour héberger des données temporaires ou facilement recréé à un coût réduit.<br><br>Adaptée aux charges de travail hautes performances où la résilience n’est pas nécessaire ou est déjà fournie par l’application.|
|**Mirror**<br><br>-Stocke deux ou trois copies des données dans l’ensemble de disques physiques<br>-Augmente la fiabilité, mais réduit la capacité. Duplication se produit avec chaque écriture. Un espace en miroir répartit également les données sur plusieurs disques physiques.<br>-Un meilleur débit et latence accès inférieure parité<br>-Utilise dirty région (DRT) pour effectuer le suivi des modifications pour les disques du pool de suivi. Lorsque le système reprend l’exécution à partir d’un arrêt non planifié et les espaces sont remis en ligne, DRT rend les disques du pool de cohérents entre eux.|Nécessite au moins deux disques physiques pour protéger contre la défaillance d’un disque.<br><br>Nécessite au moins cinq disques physiques pour protéger contre les deux défaillances simultanées de disque.|À utiliser pour la plupart des déploiements. Par exemple, les espaces de mise en miroir conviennent pour un partage de fichiers à usage général ou une bibliothèque de disque dur virtuel (VHD).|
|**Parité**<br><br>-Répartit les informations de données et la parité sur les disques physiques<br>-Augmente la fiabilité lorsqu’il est comparée à un espace simple, mais un peu réduit la capacité<br>-Augmente la résilience par le biais de la journalisation. Cela permet d’empêcher une altération des données si un arrêt non planifié se produit.|Nécessite au moins trois disques physiques pour protéger contre la défaillance d’un disque.|À utiliser pour les charges de travail qui sont hautement séquentielles, telles que l’archivage ou de sauvegarde.|

## Étape 1: Créer un pool de stockage

Vous devez premiers disques physiques disponibles de groupe dans un ou plusieurs pools de stockage.

1. Dans le volet de navigation le Gestionnaire de serveur, sélectionnez le **fichier et les Services de stockage**.

2. Dans le volet de navigation, sélectionnez la page de **Pools de stockage** .
    
    Par défaut, les disques disponibles sont inclus dans un pool qui est nommé le pool *aviaires* . Si aucun pool aviaires n’est répertorié sous **Les POOLS de stockage**, cela indique que le stockage ne répond pas à la configuration requise pour les espaces de stockage. Assurez-vous que les disques de respectent les exigences qui sont décrites dans la section des conditions préalables.
    
    >[!TIP]
    >Si vous sélectionnez le pool de stockage **aviaires** , les disques physiques disponibles sont répertoriés sous **Les disques physiques**.

3. Sous **Les POOLS de stockage**, sélectionnez la liste des **tâches** et sélectionnez **Nouveau Pool de stockage**. L’Assistant de Pool de stockage nouvelle s’ouvre.

4. Sur la page **avant de commencer** , cliquez sur **suivant**.

5. Sur la page **spécifier un nom de pool de stockage et le sous-système** , entrez un nom et une description facultative pour le pool de stockage, sélectionnez le groupe de disques physiques disponibles que vous souhaitez utiliser et sélectionnez **suivant**.

6. Sur la page **Sélectionner les disques physiques du pool de stockage** , faire le suivants et que vous sélectionnez **ensuite**:
    
    1. Activez la case à cocher en regard de chaque disque physique que vous souhaitez inclure dans le pool de stockage.
    
    2. Si vous voulez désigner une ou plusieurs disques en tant que de secours, sous l' **Allocation**, sélectionnez la flèche déroulante, puis sélectionnez **De secours**.

7. Sur la page de **confirmation sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Sur la page **Afficher les résultats** , vérifiez que toutes les tâches terminées et sélectionnez **Fermer**.
    
    >[!NOTE]
    >Si vous le souhaitez, pour passer directement à l’étape suivante, vous pouvez sélectionner la case à cocher **créer un disque virtuel lors de la fermeture de l’Assistant** .

9. Sous **Les POOLS de stockage**, vérifiez que le nouveau pool de stockage est répertorié.

### Commandes équivalentes de Windows PowerShell pour créer des pools de stockage

L’applet de commande Windows PowerShell ou les applets de commande suivantes effectuent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles pourront apparaître renvoyées sur plusieurs lignes ici en raison des contraintes de mise en forme.

L’exemple suivant montre les disques physiques sont disponibles dans le pool aviaires.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

L’exemple suivant crée un nouveau pool de stockage nommé *StoragePool1* qui utilise tous les disques disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

L’exemple suivant crée un nouveau pool de stockage, *StoragePool1*, qui utilise quatre des disques disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

La séquence d’exemple d’applets de commande suivante montre comment ajouter un disque physique disponible *PhysicalDisk5* comme un secours au pool de stockage *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## Étape 2: Créer un disque virtuel

Ensuite, vous devez créer un ou plusieurs disques virtuels du pool de stockage. Lorsque vous créez un disque virtuel, vous pouvez sélectionner la façon dont les données sont disposées sur les disques physiques. Cela affecte les performances et la fiabilité. Vous pouvez également sélectionner si vous souhaitez créer des disques ou fixe-alloué.

1. Si l’Assistant nouveau de disque virtuel n’est pas déjà ouvert, sur la page de **Pools de stockage** dans le Gestionnaire de serveur, sous **Les POOLS de stockage**, assurez-vous que le pool de stockage de votre choix est sélectionné.

2. Sous **Disques virtuels**, sélectionnez la liste des **tâches** et sélectionnez **Nouveau disque virtuel**. L’Assistant nouveau de disque virtuel s’ouvre.

3. Sur la page **avant de commencer** , cliquez sur **suivant**.

4. Sur la page **Sélectionner le pool de stockage** , sélectionnez le pool de stockage de votre choix, puis sélectionnez **suivant**.

5. Sur la page **spécifier le nom du disque virtuel** , entrez un nom et une description (facultatif), puis sélectionnez **suivant**.

6. Sur la page **Sélectionner la disposition de stockage** , sélectionnez la disposition souhaitée, puis sélectionnez **suivant**.
    
    >[!NOTE]
    >Si vous sélectionnez une disposition où vous ne disposez pas de suffisamment de disques physiques, vous recevrez un message d’erreur lorsque vous sélectionnez **suivant**. Pour plus d’informations sur la disposition à utiliser et les exigences de disque, voir [conditions préalables](#prerequisites)).

7. Si vous avez sélectionné la **mise en miroir** en tant que la disposition de stockage et que vous avez cinq ou plusieurs disques dans le pool, la page **configurer les paramètres de résilience** s’affiche. Sélectionnez l’une des options suivantes:
    
      - **Miroir double**
      - **Miroir triple**

8. Sur la page **spécifier le type de mise en service** , sélectionnez l’une des options suivantes, puis sélectionnez **suivant**.
    
      - **Léger**
        
        Avec l’allocation, l’espace est alloué sur une en fonction des besoins. Cela permet d’optimiser l’utilisation de l’espace de stockage disponible. Toutefois, dans la mesure où cela vous permet de stockage, vous devez attentivement contrôler la quantité d’espace disque est disponible.
    
      - **Fixe**
        
        Avec la mise en service fixe, la capacité de stockage est allouée immédiatement, à la création d’un disque virtuel. Par conséquent, corrigés d’approvisionnement utilise l’espace du pool de stockage qui est égal à la taille du disque virtuel.
    
    >[!TIP]
    >Avec les espaces de stockage, vous pouvez créer deux disques virtuels et fixe-alloué dans le même pool de stockage. Par exemple, vous pouvez utiliser un disque virtuel alloué pour héberger une base de données et un disque virtuel dynamiquement fixe pour héberger les fichiers journaux associés.

9. Sur la page **spécifier la taille du disque virtuel** , procédez comme suit:
    
    Si vous avez sélectionné l’allocation de l’étape précédente, dans la zone de la **taille du disque virtuel** , entrez une taille de disque virtuel, sélectionnez les unités (**Mo**, **go**ou **to**), puis sélectionnez **suivant**.
    
    Si vous avez sélectionné fixe d’approvisionnement à l’étape précédente, sélectionnez l’une des opérations suivantes:
    
      - **Spécifiez la taille**
        
        Pour spécifier une taille, entrez une valeur dans la zone de la **taille du disque virtuel** , puis sélectionnez les unités (**Mo**, **go**ou **to**).
        
        Si vous utilisez une disposition de stockage autre que simple, le disque virtuel utilise davantage d’espace libre que la taille que vous spécifiez. Pour éviter une erreur potentielle dans lequel la taille du volume est supérieure à l’espace libre de pool de stockage, vous pouvez sélectionner la case à cocher **créer le disque virtuel plus volumineux possible, jusqu'à la taille spécifiée** .
    
      - **Taille maximale**
        
        Sélectionnez cette option pour créer un disque virtuel qui utilise la capacité maximale de pool de stockage.

10. Sur la page de **confirmation sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

11. Sur la page **Afficher les résultats** , vérifiez que toutes les tâches terminées et sélectionnez **Fermer**.
    
    >[!TIP]
    >Par défaut, la case à cocher **créer un volume lors de la fermeture de l’Assistant** est sélectionnée. Vous accédez directement à l’étape suivante.

### Commandes équivalentes de Windows PowerShell pour la création de disques virtuels

L’applet de commande Windows PowerShell ou les applets de commande suivantes effectuent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles pourront apparaître renvoyées sur plusieurs lignes ici en raison des contraintes de mise en forme.

L’exemple suivant crée un disque virtuel de 50 Go nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

L’exemple suivant crée un disque virtuel en miroir nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque utilise une capacité maximale de stockage du pool stockage.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

L’exemple suivant crée un disque virtuel de 50 Go nommé *VirtualDisk1* sur un pool de stockage qui est nommé *StoragePool1*. Le disque utilise le type de mise en service légère.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

L’exemple suivant crée un disque virtuel nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque virtuel utilise la mise en miroir triple et a une taille fixe de 20 Go.

>[!NOTE]
>Vous devez disposer d’au moins cinq disques physiques dans le pool de stockage pour cette applet de commande fonctionner. (Cela n’inclut pas tous les disques qui sont attribuées en tant que disque de secours.)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## Étape 3: Créer un volume

Ensuite, vous devez créer un volume à partir du disque virtuel. Vous pouvez affecter une lettre de lecteur facultatif ou un dossier, puis formater le volume avec un système de fichiers.

1. Si l’Assistant Nouveau Volume n’est pas déjà ouvert, sur la page de **Pools de stockage** dans le Gestionnaire de serveur, sous **Disques virtuels**, cliquez sur le disque virtuel souhaité et sélectionnez **Nouveau Volume**.
    
    L’Assistant Nouveau Volume s’ouvre.

2. Sur la page **avant de commencer** , cliquez sur **suivant**.

3. Sur la page **Sélectionner le serveur et le disque** , faire le suivants et que vous sélectionnez **ensuite**.
    
    1. Dans la zone de **serveur** , sélectionnez le serveur sur lequel vous souhaitez configurer le volume.
    
    2. Dans la zone de **disque** , sélectionnez le disque virtuel sur lequel vous souhaitez créer le volume.

4. Sur la page **spécifier la taille du volume** , entrez une taille de volume, spécifiez les unités (**Mo**, **go**ou **to**) et sélectionnez **suivant**.

5. Configurer l’option voulue sur la page **affecter à une lettre de lecteur ou un dossier** et sélectionnez **suivant**.

6. Sur la page **Sélectionner les paramètres système de fichiers** , faire le suivants et que vous sélectionnez **ensuite**.
    
    1. Dans la liste de **système de fichiers** , sélectionnez **NTFS** ou **ReFS**.
    
    2. Dans la liste de la **taille d’unité d’Allocation** , moteur, conservez le paramètre **par défaut** ou définir la taille d’unité d’allocation.
        
        >[!NOTE]
        >Pour plus d’informations sur la taille d’unité d’allocation, voir la [taille de cluster pour NTFS, FAT et exFAT par défaut](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Si vous le souhaitez, dans la zone **nom de Volume** , entrez un nom d’étiquette de volume, par exemple les **Données de ressources humaines**.

7. Sur la page de **confirmation sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Sur la page **Afficher les résultats** , vérifiez que toutes les tâches terminées et sélectionnez **Fermer**.

9. Pour vérifier que le volume a été créé, dans le Gestionnaire de serveur, sélectionnez la page de **Volumes** . Le volume est répertorié sous le serveur où il a été créé. Vous pouvez également vérifier que le volume est dans l’Explorateur Windows.

### Commandes équivalentes de Windows PowerShell pour la création de volumes

L’applet de commande Windows PowerShell suivante effectue la même fonction que la procédure précédente. Entrez la commande sur une seule ligne.

L’exemple suivant initialise les disques pour le disque virtuel *VirtualDisk1*, crée une partition avec une lettre de lecteur assignée et puis met en forme le volume sur lequel le système de fichiers NTFS par défaut.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## Informations complémentaires

- [Espaces de stockage](overview.md)
- [Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Déployer des espaces de stockage en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Les espaces de stockage Forum aux Questions (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)

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
ms.openlocfilehash: 7090657a0936aed0f4b2e79007f69d7b082b0b8f
ms.sourcegitcommit: 2977c707a299929c6ab0d1e0adab2e1c644b8306
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63750651"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Déployer des espaces de stockage sur un serveur autonome

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cette rubrique décrit comment déployer des espaces de stockage sur un serveur autonome. Pour plus d’informations sur la façon de créer un espace de stockage en cluster, consultez [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Pour créer un espace de stockage, vous devez d'abord créer un ou plusieurs pools de stockage. Un pool de stockage est une collection de disques physiques. Un pool de stockage permet de regrouper le stockage, d'étendre la capacité de façon souple et de déléguer l'administration.

Vous pouvez créer un ou plusieurs disques virtuels à partir d'un pool de stockage. Ces disques virtuels sont également désignés sous le nom d'*espaces de stockage*. Un espace de stockage s'affiche dans le système d'exploitation Windows sous la forme d'un disque classique à partir duquel vous pouvez créer des volumes formatés. Quand vous créez un disque virtuel via l'interface utilisateur des services de fichiers et de stockage, vous pouvez configurer le type de résilience (simple, en miroir ou à parité), le type d'allocation (dynamique ou fixe) et la taille. À l'aide de Windows PowerShell, vous pouvez définir d'autres paramètres, tels que le nombre de colonnes, la valeur de l'entrelacement et les disques physiques du pool à utiliser. Pour plus d’informations sur ces paramètres supplémentaires, voir [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) et [Que sont les colonnes et comment les espaces de stockage décident du nombre de colonnes à utiliser ?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) dans le Forum aux Questions sur les espaces de stockage.

>[!NOTE]
>Vous ne pouvez pas utiliser un espace de stockage pour héberger le système d’exploitation Windows.

Vous pouvez créer un ou plusieurs volumes à partir d'un disque virtuel. Lorsque vous créez un volume, vous pouvez configurer la taille de la lettre de lecteur ou dossier, système de fichiers (système de fichiers NTFS ou Resilient File System (ReFS)), taille d’unité d’allocation et une étiquette de volume facultative.

La figure suivante illustre le flux de travail des espaces de stockage.

![Flux de travail des espaces de stockage](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figure 1 : Workflow d’espaces de stockage**

>[!NOTE]
>Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Prérequis

Pour utiliser des espaces de stockage sur un serveur de 2012−based Windows Server autonome, assurez-vous que les disques physiques que vous souhaitez utiliser les conditions préalables suivantes.

> [!IMPORTANT]
> Si vous souhaitez apprendre à déployer des espaces de stockage sur un cluster de basculement, consultez [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Un déploiement de cluster de basculement a des conditions préalables différentes, telles que les types de bus de disque prises en charge, les types de résilience pris en charge et le nombre minimal requis de disques.

|Domaine|Condition requise|Notes|
|---|---|---|
|Types de bus du disque|-Serial Attached SCSI (SAS)<br>-Série Advanced Technology Attachment (SATA)<br>-iSCSI et Fibre Channel contrôleurs. |Vous pouvez également utiliser des lecteurs USB. Toutefois, il n’est pas optimal pour utiliser des lecteurs USB dans un environnement serveur.<br>Espaces de stockage est pris en charge iSCSI et Fibre Channel (FC) contrôleurs tant que les disques virtuels créés sur les sont non résilient (Simple avec n’importe quel nombre de colonnes).<br>|
|Configuration de disque|-Disques physiques doivent être au moins 4 Go<br>-Les disques doivent être vierges et non formatés. Ne créez pas de volumes.||
|Considérations relatives aux adaptateurs de bus hôte|-Adaptateurs de bus hôte simple (HBA) qui ne prennent pas en charge les fonctionnalités RAID sont recommandés.<br>-Si RAID prenant en charge, les adaptateurs de bus hôte doivent être en mode de non RAID avec toutes les fonctionnalités RAID désactivées<br>-Les adaptateurs ne doivent pas abstraire les disques physiques, les données du cache, ou masquer les périphériques connectés. Cela comprend les services de boîtier fournis par les périphériques JBOD (just-a-bunch-of-disks) connectés. |Les espaces de stockage sont compatibles uniquement avec les adaptateurs de bus hôte où vous pouvez désactiver intégralement toutes les fonctionnalités RAID.|
|Boîtiers JBoD|-Boîtiers JBOD sont facultatifs<br>-Recommandé d’utiliser des espaces de stockage certifié boîtiers répertoriés dans le catalogue Windows Server<br>-Si vous utilisez un boîtier JBOD, vérifiez avec votre fournisseur de stockage que le boîtier prend en charge les espaces de stockage pour garantir la fonctionnalité complète<br>-Pour déterminer si le boîtier JBOD prend en charge l’identification des boîtiers et emplacements, exécutez l’applet de commande Windows PowerShell suivante :<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si le **EnclosureNumber** et **SlotNumber** champs contiennent des valeurs, puis le boîtier prend en charge ces fonctionnalités.||

Pour planifier le nombre de disques physiques et le type de résilience voulu pour le déploiement d'un serveur autonome, utilisez les instructions suivantes.

|Type de résilience|Configuration requise pour le disque|Quand l’utiliser|
|---|---|---|
|**Simple**<br><br>-Agrège les données sur des disques physiques<br>-Optimise la capacité du disque et augmente le débit<br>-Aucune résilience (ne protège pas contre une défaillance de disque)<br><br><br><br><br><br><br>|Nécessite au moins un disque physique.|À ne pas utiliser pour héberger des données irremplaçables. Espaces simples ne protègent pas contre les défaillances de disque.<br><br>À utiliser pour héberger des données temporaires ou facilement recréées à moindre coût.<br><br>Adapté aux charges de travail hautes performances où la résilience n’est pas nécessaire ou est déjà fournie par l’application.|
|**Miroir**<br><br>-Stocke deux ou trois copies des données sur l’ensemble des disques physiques<br>-Améliore la fiabilité mais réduit la capacité. Une duplication se produit avec chaque écriture. Un espace en miroir agrège également les données par bandes sur plusieurs disques physiques.<br>-Un meilleur débit de données et la latence d’accès inférieure que la parité<br>-Utilise dirty région DRT () pour effectuer le suivi des modifications des disques dans le pool de suivi. Quand le système redémarre après un arrêt non planifié et que les espaces sont remis en ligne, DRT assure la cohérence des disques dans le pool.|Nécessite au moins deux disques physiques pour une protection contre une seule défaillance de disque.<br><br>Nécessite au moins cinq disques physiques pour une protection contre deux défaillances de disque simultanées.|À utiliser pour la plupart des déploiements. Par exemple, les espaces en miroir sont adaptés à un partage de fichiers à usage général ou à une bibliothèque de disques durs virtuels (VHD).|
|**Parité**<br><br>-Répartit les informations de données et la parité sur les disques physiques<br>-Améliore la fiabilité lorsqu’il est comparé à un espace simple, mais réduit quelque peu la capacité<br>-Augmente la résilience par la journalisation. Vous pouvez ainsi empêcher tout endommagement des données en cas d'arrêt non planifié.|Nécessite au moins trois disques physiques pour une protection contre une seule défaillance de disque.|À utiliser pour les charges de travail hautement séquentielles, telles que l'archivage ou la sauvegarde.|

## <a name="step-1-create-a-storage-pool"></a>Étape 1 : Créer un pool de stockage

Vous devez d'abord regrouper les disques physiques disponibles en un ou plusieurs pools de stockage.

1. Dans le volet de navigation de gestionnaire de serveur, sélectionnez **File and Storage Services**.

2. Dans le volet de navigation, sélectionnez le **Pools de stockage** page.
    
    Par défaut, les disques disponibles sont inclus dans un pool appelé pool *primordial*. Si aucun pool primordial n'est répertorié sous **POOLS DE STOCKAGE**, cela indique que le stockage ne répond pas aux conditions requises pour les espaces de stockage. Assurez-vous que les disques répondent aux conditions requises qui sont présentées dans la section Conditions préalables.
    
    >[!TIP]
    >Si vous sélectionnez le pool de stockage **Primordial**, les disques physiques disponibles sont répertoriés sous **DISQUES PHYSIQUES**.

3. Sous **POOLS de stockage**, sélectionnez le **tâches** liste, puis sélectionnez **nouveau Pool de stockage**. L’Assistant Nouveau Pool de stockage s’ouvre.

4. Sur le **avant de commencer** page, sélectionnez **suivant**.

5. Sur le **spécifier un nom de pool de stockage et le sous-système** page, entrez un nom et une description facultative pour le pool de stockage, sélectionnez le groupe de disques physiques disponibles que vous souhaitez utiliser, puis sélectionnez **suivant**.

6. Sur le **sélectionner les disques physiques du pool de stockage** page, procédez comme suit, puis sélectionnez **suivant**:
    
    1. Cochez la case en regard de chaque disque physique à inclure dans le pool de stockage.
    
    2. Si vous souhaitez désigner un ou plusieurs disques comme disques d’échange à chaud, sous **Allocation**, sélectionnez la flèche déroulante, puis sélectionnez **échange à chaud**.

7. Sur le **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Sur le **afficher les résultats** , vérifiez que toutes les tâches terminées, puis sélectionnez **fermer**.
    
    >[!NOTE]
    >Éventuellement, pour passer directement à l'étape suivante, vous pouvez cocher la case **Créer un disque virtuel lorsque l'Assistant se ferme**.

9. Sous **POOLS DE STOCKAGE**, vérifiez que le nouveau pool de stockage est répertorié.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Commandes équivalentes de Windows PowerShell pour la création de pools de stockage

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L'exemple suivant indique les disques physiques qui sont disponibles dans le pool primordial.

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

L'exemple de séquence d'applets de commande suivant montre comment ajouter un disque physique disponible *PhysicalDisk5* en tant que disque d'échange à chaud au pool de stockage *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Étape 2 : Créer un disque virtuel

Vous devez ensuite créer un ou plusieurs disques virtuels à partir du pool de stockage. Quand vous créez un disque virtuel, vous pouvez sélectionner la disposition des données sur les disques physiques. Cela affecte à la fois la fiabilité et les performances. Vous pouvez également choisir de créer des disques à allocation dynamique ou fixe.

1. Si l'Assistant Nouveau disque virtuel n'est pas déjà ouvert, dans la page **Pools de stockage** du Gestionnaire de serveur, sous **POOLS DE STOCKAGE**, assurez-vous que le pool de stockage voulu est sélectionné.

2. Sous **disques virtuels**, sélectionnez le **tâches** liste, puis sélectionnez **nouveau disque virtuel**. L’Assistant Création de disque virtuel s’ouvre.

3. Sur le **avant de commencer** page, sélectionnez **suivant**.

4. Sur le **sélectionner le pool de stockage** page, sélectionnez le pool de stockage souhaité, puis **suivant**.

5. Sur le **spécifier le nom du disque virtuel** page, entrez un nom et une description facultative, puis sélectionnez **suivant**.

6. Sur le **sélectionner la disposition de stockage** page, sélectionnez la disposition souhaitée, puis sélectionnez **suivant**.
    
    >[!NOTE]
    >Si vous sélectionnez une mise en page où vous n’avez pas suffisamment de disques physiques, vous recevrez un message d’erreur lorsque vous sélectionnez **suivant**. Pour plus d’informations sur la disposition à des conditions d’utilisation et les disque, consultez [conditions préalables](#prerequisites)).

7. Si vous avez sélectionné **miroir** comme la disposition de stockage et que vous avez au moins cinq disques dans le pool, le **configurer les paramètres de résilience** page s’affiche. Vous pouvez sélectionner l'une des options suivantes :
    
      - **Miroir double**
      - **Miroir triple**

8. Sur le **spécifier le type d’approvisionnement** page, sélectionnez une des options suivantes, puis sélectionnez **suivant**.
    
      - **Dynamique**
        
        Avec l'allocation dynamique, l'espace est alloué en fonction des besoins. L'utilisation du stockage disponible est ainsi optimisée. Toutefois, comme cela vous permet d'allouer le stockage de façon excessive, vous devez soigneusement surveiller la quantité d'espace disque disponible.
    
      - **fixe**
        
        Avec l'allocation fixe, la capacité de stockage est allouée immédiatement, au moment de la création d'un disque virtuel. Par conséquent, l'allocation fixe utilise une quantité d'espace du pool de stockage égale à la taille du disque virtuel.
    
    >[!TIP]
    >Avec les espaces de stockage, vous pouvez créer à la fois des disques virtuels à allocation dynamique et fixe dans le même pool de stockage. Par exemple, vous pouvez utiliser un disque virtuel à allocation dynamique pour héberger une base de données et un disque virtuel à allocation fixe pour héberger les fichiers journaux associés.

9. Dans la page **Spécifier la taille du disque virtuel**, procédez comme suit :
    
    Si vous avez sélectionné l’allocation dynamique à l’étape précédente, dans le **taille du disque virtuel** , entrez une taille de disque virtuel, sélectionnez les unités (**Mo**, **Go**, ou **to** ), puis sélectionnez **suivant**.
    
    Si vous avez sélectionné l’allocation fixe à l’étape précédente, sélectionnez une des opérations suivantes :
    
      - **Spécifier la taille**
        
        Pour spécifier une taille, entrez une valeur dans le **taille du disque virtuel** zone, puis sélectionnez les unités (**Mo**, **Go**, ou **to**).
        
        Si vous utilisez une disposition de stockage autre que la disposition simple, le disque virtuel utilise plus d'espace libre que la taille que vous spécifiez. Pour éviter une erreur potentielle si la taille du volume dépasse l'espace libre du pool de stockage, vous pouvez cocher la case **Créer le disque virtuel le plus volumineux possible par rapport à la taille spécifiée**.
    
      - **Taille maximale**
        
        Sélectionnez cette option pour créer un disque virtuel qui utilise la capacité maximale du pool de stockage.

10. Sur le **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

11. Sur le **afficher les résultats** , vérifiez que toutes les tâches terminées, puis sélectionnez **fermer**.
    
    >[!TIP]
    >Par défaut, la case **Créer un volume lorsque l'Assistant se ferme** est cochée. Vous passez directement à l'étape suivante.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Commandes équivalentes de Windows PowerShell pour la création de disques virtuels

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L’exemple suivant crée un disque virtuel de 50 Go nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

L’exemple suivant crée un disque virtuel en miroir nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque utilise la capacité de stockage maximale du pool de stockage.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –ResiliencySettingName Mirror –UseMaximumSize
```

L’exemple suivant crée un disque virtuel de 50 Go nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque utilise le type d'allocation dynamique.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB) –ProvisioningType Thin
```

L’exemple suivant crée un disque virtuel nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque virtuel utilise une mise en miroir triple et a une taille fixe de 20 Go.

>[!NOTE]
>Vous devez disposer d'au moins cinq disques physiques dans le pool de stockage pour que cette applet de commande fonctionne. (Les disques alloués en tant que disques d'échange à chaud ne sont pas inclus.)

```PowerShell
New-VirtualDisk -StoragePoolFriendlyName StoragePool1 -FriendlyName VirtualDisk1 -ResiliencySettingName Mirror -NumberOfDataCopies 3 -Size 20GB -ProvisioningType Fixed
```

## <a name="step-3-create-a-volume"></a>Étape 3 : Créer un volume

Vous devez ensuite créer un volume à partir du disque virtuel. Vous pouvez affecter une lettre de lecteur facultative ou un dossier, puis formater le volume avec un système de fichiers.

1. Si l’Assistant Nouveau Volume n’est pas déjà ouvert, dans le **Pools de stockage** page dans le Gestionnaire de serveur, sous **disques virtuels**, cliquez sur le disque virtuel voulu, puis sélectionnez **nouveau Volume**.
    
    L'Assistant Création d'un nouveau volume s'ouvre.

2. Sur le **avant de commencer** page, sélectionnez **suivant**.

3. Sur le **sélectionner le serveur et le disque** page, procédez comme suit, puis sélectionnez **suivant**.
    
    1. Dans le **Server** zone, sélectionnez le serveur sur lequel vous souhaitez configurer le volume.
    
    2. Dans le **disque** zone, sélectionnez le disque virtuel sur lequel vous souhaitez créer le volume.

4. Sur le **spécifier la taille du volume** , entrez une taille de volume, spécifiez le nombre d’unités (**Mo**, **Go**, ou **to**), puis sélectionnez **Suivant**.

5. Sur le **affecter à une lettre de lecteur ou le dossier** page, configurez l’option souhaitée, puis sélectionnez **suivant**.

6. Sur le **sélectionner des paramètres système de fichiers** page, procédez comme suit, puis sélectionnez **suivant**.
    
    1. Dans le **système de fichiers** , sélectionnez soit **NTFS** ou **ReFS**.
    
    2. Dans la liste **Taille d'unité d'allocation**, laissez la valeur de paramètre **Par défaut** ou définissez la taille d'unité d'allocation.
        
        >[!NOTE]
        >Pour plus d’informations sur la taille d’unité d’allocation, consultez [Taille de cluster par défaut pour NTFS, FAT et exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Vous pouvez éventuellement, dans la zone **Nom de volume**, entrer un nom d'étiquette de volume, par exemple **Données RH**.

7. Sur le **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Sur le **afficher les résultats** , vérifiez que toutes les tâches terminées, puis sélectionnez **fermer**.

9. Pour vérifier que le volume a été créé, dans le Gestionnaire de serveur, sélectionnez le **Volumes** page. Le volume est répertorié sous le serveur où il a été créé. Vous pouvez également vérifier que le volume figure dans l'Explorateur Windows.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Commandes équivalentes de Windows PowerShell pour la création de volumes

L’applet de commande Windows PowerShell suivante effectue la même fonction que la procédure précédente. Entrez la commande sur une seule ligne.

L'exemple suivant initialise les disques pour le disque virtuel *VirtualDisk1*, crée une partition avec une lettre de lecteur affectée, puis formate le volume avec le système de fichiers NTFS par défaut.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Informations complémentaires

- [Espaces de stockage](overview.md)
- [Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Déployer des espaces de stockage en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Forum aux Questions (FAQ sur) les espaces de stockage](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)

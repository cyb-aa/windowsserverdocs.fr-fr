---
title: Déployer des espaces de stockage sur un serveur autonome
description: Décrit comment déployer des espaces de stockage sur un serveur Windows Server 2012 autonome.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-spaces
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 52e6a4d53271a73bc0913e2ac500c4328f2e7009
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393732"
---
# <a name="deploy-storage-spaces-on-a-stand-alone-server"></a>Déployer des espaces de stockage sur un serveur autonome

>S’applique à : Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 et Windows Server 2012

Cette rubrique explique comment déployer des espaces de stockage sur un serveur autonome. Pour plus d’informations sur la création d’un espace de stockage en cluster, voir [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>).

Pour créer un espace de stockage, vous devez d'abord créer un ou plusieurs pools de stockage. Un pool de stockage est une collection de disques physiques. Un pool de stockage permet de regrouper le stockage, d'étendre la capacité de façon souple et de déléguer l'administration.

Vous pouvez créer un ou plusieurs disques virtuels à partir d'un pool de stockage. Ces disques virtuels sont également désignés sous le nom d'*espaces de stockage*. Un espace de stockage s'affiche dans le système d'exploitation Windows sous la forme d'un disque classique à partir duquel vous pouvez créer des volumes formatés. Quand vous créez un disque virtuel via l'interface utilisateur des services de fichiers et de stockage, vous pouvez configurer le type de résilience (simple, en miroir ou à parité), le type d'allocation (dynamique ou fixe) et la taille. À l'aide de Windows PowerShell, vous pouvez définir d'autres paramètres, tels que le nombre de colonnes, la valeur de l'entrelacement et les disques physiques du pool à utiliser. Pour plus d’informations sur ces paramètres supplémentaires, voir [New-VirtualDisk](https://docs.microsoft.com/powershell/module/storage/new-virtualdisk?view=win10-ps) et [Que sont les colonnes et comment les espaces de stockage décident du nombre de colonnes à utiliser ?](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx%23what_are_columns_and_how_does_storage_spaces_decide_how_many_to_use) dans le Forum aux Questions sur les espaces de stockage.

>[!NOTE]
>Vous ne pouvez pas utiliser un espace de stockage pour héberger le système d’exploitation Windows.

Vous pouvez créer un ou plusieurs volumes à partir d'un disque virtuel. Lorsque vous créez un volume, vous pouvez configurer la taille, la lettre de lecteur ou le dossier, le système de fichiers (système de fichiers NTFS ou le système de fichiers résilient (ReFS)), la taille d’unité d’allocation et une étiquette de volume facultative.

La figure suivante illustre le flux de travail des espaces de stockage.

![Flux de travail des espaces de stockage](media/deploy-standalone-storage-spaces/storage-spaces-workflow.png)

**Figure 1 : flux de travail des espaces de stockage**

>[!NOTE]
>Cette rubrique inclut des exemples d'applets de commande Windows PowerShell que vous pouvez utiliser pour automatiser certaines des procédures décrites. Pour plus d’informations, consultez [PowerShell](https://docs.microsoft.com/powershell/scripting/powershell-scripting?view=powershell-6).

## <a name="prerequisites"></a>Conditions préalables

Pour utiliser des espaces de stockage sur un serveur autonome Windows Server 2012, assurez-vous que les disques physiques que vous voulez utiliser respectent les conditions préalables suivantes.

> [!IMPORTANT]
> Si vous souhaitez savoir comment déployer des espaces de stockage sur un cluster de basculement, voir [déployer un cluster d’espaces de stockage sur Windows Server 2012 R2](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/mt270997(v%3dws.11)>). Un déploiement de cluster de basculement présente des conditions préalables différentes, telles que les types de bus de disque pris en charge, les types de résilience pris en charge et le nombre minimal de disques requis.

|Zone|Condition requise|Remarques|
|---|---|---|
|Types de bus du disque|-SAS (Serial Attached SCSI)<br>-SATA (Serial Advanced Technology Attachment)<br>-les contrôleurs iSCSI et Fibre Channel. |Vous pouvez également utiliser des lecteurs USB. Toutefois, il n’est pas optimal d’utiliser des lecteurs USB dans un environnement de serveur.<br>Les espaces de stockage sont pris en charge sur les contrôleurs iSCSI et Fibre Channel (FC) tant que les disques virtuels créés sur ceux-ci ne sont pas résilients (simples avec un nombre quelconque de colonnes).<br>|
|Configuration de disque|-Les disques physiques doivent avoir au moins 4 Go<br>-Les disques doivent être vierges et non formatés. Ne créez pas de volumes.||
|Considérations relatives aux adaptateurs de bus hôte|-Les adaptateurs de bus hôte (HBA) simples qui ne prennent pas en charge la fonctionnalité RAID sont recommandés<br>-En cas de compatibilité RAID, les HBA doivent être en mode non RAID avec toutes les fonctionnalités RAID désactivées<br>-Les adaptateurs ne doivent pas abstraire les disques physiques, mettre en cache les données ni masquer les périphériques attachés. Cela comprend les services de boîtier fournis par les périphériques JBOD (just-a-bunch-of-disks) connectés. |Les espaces de stockage sont compatibles uniquement avec les adaptateurs de bus hôte où vous pouvez désactiver intégralement toutes les fonctionnalités RAID.|
|Boîtiers JBoD|-Les boîtiers JBOD sont facultatifs<br>-Recommandé d’utiliser les boîtiers d’espaces de stockage certifiés répertoriés dans le catalogue Windows Server<br>-Si vous utilisez un boîtier JBOD, vérifiez auprès de votre fournisseur de stockage que le boîtier prend en charge les espaces de stockage pour garantir l’intégralité des fonctionnalités<br>-Pour déterminer si le boîtier JBOD prend en charge l’identification des boîtiers et des emplacements, exécutez l’applet de commande Windows PowerShell suivante :<br><br>`Get-PhysicalDisk \| ? {$_.BusType –eq "SAS"} \| fc`<br><br>Si les champs **EnclosureNumber** et **SlotNumber** contiennent des valeurs, le boîtier prend en charge ces fonctionnalités.||

Pour planifier le nombre de disques physiques et le type de résilience voulu pour le déploiement d'un serveur autonome, utilisez les instructions suivantes.

|Type de résilience|Configuration requise pour le disque|Quand l’utiliser|
|---|---|---|
|**Simple**<br><br>-Entrelace les données sur les disques physiques<br>-Optimise la capacité du disque et augmente le débit<br>-Aucune résilience (ne protège pas contre les défaillances de disque)<br><br><br><br><br><br><br>|Nécessite au moins un disque physique.|À ne pas utiliser pour héberger des données irremplaçables. Les espaces simples ne protègent pas les défaillances de disque.<br><br>À utiliser pour héberger des données temporaires ou facilement recréées à moindre coût.<br><br>Adapté aux charges de travail hautes performances dans lesquelles la résilience n’est pas requise ou est déjà fournie par l’application.|
|**Miroir**<br><br>-Stocke deux ou trois copies des données sur l’ensemble des disques physiques.<br>-Augmente la fiabilité, mais réduit la capacité. Une duplication se produit avec chaque écriture. Un espace en miroir agrège également les données par bandes sur plusieurs disques physiques.<br>-Meilleur débit de données et latence d’accès inférieure à la parité<br>-Utilise le suivi des régions modifiées (DRT) pour effectuer le suivi des modifications apportées aux disques dans le pool. Quand le système redémarre après un arrêt non planifié et que les espaces sont remis en ligne, DRT assure la cohérence des disques dans le pool.|Nécessite au moins deux disques physiques pour une protection contre une seule défaillance de disque.<br><br>Nécessite au moins cinq disques physiques pour une protection contre deux défaillances de disque simultanées.|À utiliser pour la plupart des déploiements. Par exemple, les espaces en miroir sont adaptés à un partage de fichiers à usage général ou à une bibliothèque de disques durs virtuels (VHD).|
|**Parité**<br><br>-Entrelace les données et les informations de parité sur les disques physiques<br>-Augmente la fiabilité lorsqu’il est comparé à un espace simple, mais réduit la capacité<br>-Augmente la résilience par le biais de la journalisation. Vous pouvez ainsi empêcher tout endommagement des données en cas d'arrêt non planifié.|Nécessite au moins trois disques physiques pour une protection contre une seule défaillance de disque.|À utiliser pour les charges de travail hautement séquentielles, telles que l'archivage ou la sauvegarde.|

## <a name="step-1-create-a-storage-pool"></a>Étape 1 : créer un pool de stockage

Vous devez d'abord regrouper les disques physiques disponibles en un ou plusieurs pools de stockage.

1. Dans le volet de navigation Gestionnaire de serveur, sélectionnez **services de fichiers et de stockage**.

2. Dans le volet de navigation, sélectionnez la page **pools de stockage** .
    
    Par défaut, les disques disponibles sont inclus dans un pool appelé pool *primordial*. Si aucun pool primordial n'est répertorié sous **POOLS DE STOCKAGE**, cela indique que le stockage ne répond pas aux conditions requises pour les espaces de stockage. Assurez-vous que les disques répondent aux conditions requises qui sont présentées dans la section Conditions préalables.
    
    >[!TIP]
    >Si vous sélectionnez le pool de stockage **Primordial**, les disques physiques disponibles sont répertoriés sous **DISQUES PHYSIQUES**.

3. Sous **pools de stockage**, sélectionnez la liste **tâches** , puis sélectionnez **nouveau pool de stockage**. L’assistant nouveau pool de stockage s’ouvre.

4. Dans la page **avant de commencer** , sélectionnez **suivant**.

5. Dans la page **spécifier un nom et un sous-système de pool de stockage** , entrez un nom et une description facultative pour le pool de stockage, sélectionnez le groupe de disques physiques disponibles que vous souhaitez utiliser, puis sélectionnez **suivant**.

6. Dans la page **Sélectionner les disques physiques pour le pool de stockage** , effectuez les opérations suivantes, puis sélectionnez **suivant**:
    
    1. Cochez la case en regard de chaque disque physique à inclure dans le pool de stockage.
    
    2. Si vous souhaitez désigner un ou plusieurs disques en tant que disques d’échange à chaud, sous **allocation**, sélectionnez la flèche déroulante, puis sélectionnez **disque d’échange à chaud**.

7. Sur la page **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Dans la page **afficher les résultats** , vérifiez que toutes les tâches sont terminées, puis sélectionnez **Fermer**.
    
    >[!NOTE]
    >Éventuellement, pour passer directement à l'étape suivante, vous pouvez cocher la case **Créer un disque virtuel lorsque l'Assistant se ferme**.

9. Sous **POOLS DE STOCKAGE**, vérifiez que le nouveau pool de stockage est répertorié.

### <a name="windows-powershell-equivalent-commands-for-creating-storage-pools"></a>Commandes Windows PowerShell équivalentes pour la création de pools de stockage

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L'exemple suivant indique les disques physiques qui sont disponibles dans le pool primordial.

```PowerShell
Get-StoragePool -IsPrimordial $true | Get-PhysicalDisk | Where-Object CanPool -eq $True
```

L’exemple suivant crée un pool de stockage nommé *StoragePool1* qui utilise tous les disques disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk –CanPool $True)
```

L’exemple suivant crée un pool de stockage, *StoragePool1*, qui utilise quatre des disques disponibles.

```PowerShell
New-StoragePool –FriendlyName StoragePool1 –StorageSubsystemFriendlyName “Storage Spaces*” –PhysicalDisks (Get-PhysicalDisk PhysicalDisk1, PhysicalDisk2, PhysicalDisk3, PhysicalDisk4)
```

L'exemple de séquence d'applets de commande suivant montre comment ajouter un disque physique disponible *PhysicalDisk5* en tant que disque d'échange à chaud au pool de stockage *StoragePool1*.

```PowerShell
$PDToAdd = Get-PhysicalDisk –FriendlyName PhysicalDisk5
Add-PhysicalDisk –StoragePoolFriendlyName StoragePool1 –PhysicalDisks $PDToAdd –Usage HotSpare
```

## <a name="step-2-create-a-virtual-disk"></a>Étape 2 : créer un disque virtuel.

Vous devez ensuite créer un ou plusieurs disques virtuels à partir du pool de stockage. Quand vous créez un disque virtuel, vous pouvez sélectionner la disposition des données sur les disques physiques. Cela affecte à la fois la fiabilité et les performances. Vous pouvez également choisir de créer des disques à allocation dynamique ou fixe.

1. Si l'Assistant Nouveau disque virtuel n'est pas déjà ouvert, dans la page **Pools de stockage** du Gestionnaire de serveur, sous **POOLS DE STOCKAGE**, assurez-vous que le pool de stockage voulu est sélectionné.

2. Sous **disques virtuels**, sélectionnez la liste **tâches** , puis sélectionnez **nouveau disque virtuel**. L’assistant nouveau disque virtuel s’ouvre.

3. Dans la page **avant de commencer** , sélectionnez **suivant**.

4. Dans la page **Sélectionner le pool de stockage** , sélectionnez le pool de stockage souhaité, puis sélectionnez **suivant**.

5. Dans la page **spécifier le nom du disque virtuel** , entrez un nom et éventuellement une description, puis sélectionnez **suivant**.

6. Dans la page **Sélectionner la disposition de stockage** , sélectionnez la disposition souhaitée, puis sélectionnez **suivant**.
    
    >[!NOTE]
    >Si vous sélectionnez une disposition dans laquelle vous n’avez pas assez de disques physiques, un message d’erreur s’affiche lorsque vous sélectionnez **suivant**. Pour plus d’informations sur la disposition à utiliser et les disques requis, voir [conditions préalables requises](#prerequisites).

7. Si vous avez sélectionné **miroir** comme disposition de stockage et que vous disposez de cinq disques ou plus dans le pool, la page **configurer les paramètres de résilience** s’affiche. Vous pouvez sélectionner l'une des options suivantes :
    
      - **Miroir double**
      - **Miroir triple**

8. Dans la page **spécifier le type de provisionnement** , sélectionnez l’une des options suivantes, puis sélectionnez **suivant**.
    
   - **Dynamique**
        
     Avec l'allocation dynamique, l'espace est alloué en fonction des besoins. L'utilisation du stockage disponible est ainsi optimisée. Toutefois, comme cela vous permet d'allouer le stockage de façon excessive, vous devez soigneusement surveiller la quantité d'espace disque disponible.
    
   - **Des**
        
     Avec l'allocation fixe, la capacité de stockage est allouée immédiatement, au moment de la création d'un disque virtuel. Par conséquent, l'allocation fixe utilise une quantité d'espace du pool de stockage égale à la taille du disque virtuel.
    
     >[!TIP]
     >Avec les espaces de stockage, vous pouvez créer à la fois des disques virtuels à allocation dynamique et fixe dans le même pool de stockage. Par exemple, vous pouvez utiliser un disque virtuel à allocation dynamique pour héberger une base de données et un disque virtuel à allocation fixe pour héberger les fichiers journaux associés.

9. Dans la page **Spécifier la taille du disque virtuel**, procédez comme suit :
    
    Si vous avez sélectionné l’allocation dynamique à l’étape précédente, dans la **zone Taille du disque virtuel** , entrez une taille de disque virtuel, sélectionnez les unités (**Mo**, **Go**ou **to**), puis sélectionnez **suivant**.
    
    Si vous avez sélectionné l’approvisionnement fixe à l’étape précédente, sélectionnez l’une des options suivantes :
    
      - **Spécifier la taille**
        
        Pour spécifier une taille, entrez une valeur dans la zone **taille du disque virtuel** , puis sélectionnez les unités (**Mo**, **Go**ou **to**).
        
        Si vous utilisez une disposition de stockage autre que la disposition simple, le disque virtuel utilise plus d'espace libre que la taille que vous spécifiez. Pour éviter une erreur potentielle si la taille du volume dépasse l'espace libre du pool de stockage, vous pouvez cocher la case **Créer le disque virtuel le plus volumineux possible par rapport à la taille spécifiée**.
    
      - **Taille maximale**
        
        Sélectionnez cette option pour créer un disque virtuel qui utilise la capacité maximale du pool de stockage.

10. Sur la page **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

11. Dans la page **afficher les résultats** , vérifiez que toutes les tâches sont terminées, puis sélectionnez **Fermer**.
    
    >[!TIP]
    >Par défaut, la case **Créer un volume lorsque l'Assistant se ferme** est cochée. Vous passez directement à l'étape suivante.

### <a name="windows-powershell-equivalent-commands-for-creating-virtual-disks"></a>Commandes Windows PowerShell équivalentes pour la création de disques virtuels

L'applet ou les applets de commande Windows PowerShell suivantes remplissent la même fonction que la procédure précédente. Entrez chaque applet de commande sur une seule ligne, même si elles peuvent apparaître comme renvoyées sur plusieurs lignes ici en raison de contraintes de mise en forme.

L’exemple suivant crée un disque virtuel de 50 Go nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*.

```PowerShell
New-VirtualDisk –StoragePoolFriendlyName StoragePool1 –FriendlyName VirtualDisk1 –Size (50GB)
```

L’exemple suivant crée un disque virtuel mis en miroir nommé *VirtualDisk1* sur un pool de stockage nommé *StoragePool1*. Le disque utilise la capacité de stockage maximale du pool de stockage.

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

## <a name="step-3-create-a-volume"></a>Étape 3 : créer un volume

Vous devez ensuite créer un volume à partir du disque virtuel. Vous pouvez affecter une lettre de lecteur ou un dossier facultatif, puis formater le volume avec un système de fichiers.

1. Si l’Assistant Création d’un nouveau volume n’est pas déjà ouvert, dans la page **pools de stockage** de gestionnaire de serveur, sous **disques virtuels**, cliquez avec le bouton droit sur le disque virtuel de votre choix, puis sélectionnez **nouveau volume**.
    
    L'Assistant Création d'un nouveau volume s'ouvre.

2. Dans la page **avant de commencer** , sélectionnez **suivant**.

3. Dans la page **Sélectionner le serveur et le disque** , effectuez les opérations suivantes, puis sélectionnez **suivant**.
    
    1. Dans la zone **serveur** , sélectionnez le serveur sur lequel vous souhaitez approvisionner le volume.
    
    2. Dans la zone **disque** , sélectionnez le disque virtuel sur lequel vous souhaitez créer le volume.

4. Dans la page **spécifier la taille du volume** , entrez une taille de volume, spécifiez les unités **(Mo**, **Go**ou **to**), puis sélectionnez **suivant**.

5. Dans la page **affecter à une lettre de lecteur ou à un dossier** , configurez l’option souhaitée, puis sélectionnez **suivant**.

6. Dans la page **Sélectionner les paramètres du système de fichiers** , effectuez les opérations suivantes, puis sélectionnez **suivant**.
    
    1. Dans la liste **système de fichiers** , sélectionnez **NTFS** ou **ReFS**.
    
    2. Dans la liste **Taille d'unité d'allocation**, laissez la valeur de paramètre **Par défaut** ou définissez la taille d'unité d'allocation.
        
        >[!NOTE]
        >Pour plus d’informations sur la taille d’unité d’allocation, consultez [Taille de cluster par défaut pour NTFS, FAT et exFAT](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat).

    
    3. Vous pouvez éventuellement, dans la zone **Nom de volume**, entrer un nom d'étiquette de volume, par exemple **Données RH**.

7. Sur la page **confirmer les sélections** , vérifiez que les paramètres sont corrects, puis sélectionnez **créer**.

8. Dans la page **afficher les résultats** , vérifiez que toutes les tâches sont terminées, puis sélectionnez **Fermer**.

9. Pour vérifier que le volume a été créé, dans Gestionnaire de serveur, sélectionnez la page **volumes** . Le volume est répertorié sous le serveur où il a été créé. Vous pouvez également vérifier que le volume figure dans l'Explorateur Windows.

### <a name="windows-powershell-equivalent-commands-for-creating-volumes"></a>Commandes Windows PowerShell équivalentes pour la création de volumes

L’applet de commande Windows PowerShell suivante effectue la même fonction que la procédure précédente. Entrez la commande sur une seule ligne.

L'exemple suivant initialise les disques pour le disque virtuel *VirtualDisk1*, crée une partition avec une lettre de lecteur affectée, puis formate le volume avec le système de fichiers NTFS par défaut.

```PowerShell
Get-VirtualDisk –FriendlyName VirtualDisk1 | Get-Disk | Initialize-Disk –Passthru | New-Partition –AssignDriveLetter –UseMaximumSize | Format-Volume
```

## <a name="additional-information"></a>Informations complémentaires

- [Espaces de stockage](overview.md)
- [Applets de commande de stockage dans Windows PowerShell](https://docs.microsoft.com/powershell/module/storage/index?view=win10-ps)
- [Déployer des espaces de stockage en cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11))
- [Espaces de stockage-Forum aux questions (FAQ)](https://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)

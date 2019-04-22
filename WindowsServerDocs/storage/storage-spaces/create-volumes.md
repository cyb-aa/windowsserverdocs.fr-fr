---
ms.assetid: a9f229eb-bef4-4231-97d0-0899e17cef32
title: Création de volumes dans les espaces de stockage direct
ms.prod: windows-server-threshold
ms.author: cosdar
ms.manager: eldenc
ms.technology: storage-spaces
ms.topic: article
author: cosmosdarwin
ms.date: 01/11/2017
ms.localizationpriority: medium
ms.openlocfilehash: 277a676d8e53a7847d54039aab6607be8e5a78c5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823610"
---
# <a name="creating-volumes-in-storage-spaces-direct"></a>Création de volumes dans les espaces de stockage direct

>S'applique à : Windows Server 2016

Cette rubrique explique comment créer des volumes dans les espaces de stockage direct à l’aide de PowerShell ou du Gestionnaire du cluster de basculement.

   >[!TIP]
   >  Si vous ne l'avez pas encore fait, consultez d'abord [Planification des volumes dans les espaces de stockage direct](plan-volumes.md).

## <a name="create-volumes-using-powershell"></a>Créer des volumes à l’aide de PowerShell

Nous vous recommandons d’utiliser l'applet de commande **New-Volume** pour créer des volumes pour les espaces de stockage direct. Il fournit l’expérience la plus rapide et la plus simple. Cet applet de commande unique crée et formate automatiquement le disque virtuel et les partitions, crée le volume avec le nom correspondant et l’ajoute aux volumes partagés de cluster – le tout en une seule étape facile.

L'applet de commande **New-Volume** inclut quatre paramètres que vous devez toujours renseigner :

- **FriendlyName :** N’importe quelle chaîne de votre choix, par exemple *« Volume1 »*
- **Système de fichiers :** Soit **CSVFS_ReFS** (recommandé) ou **CSVFS_NTFS**
- **StoragePoolFriendlyName :** Le nom de votre espace de stockage du pool, par exemple *« S2D sur ClusterName »*
- **Taille :** La taille du volume, par exemple *« 10 To »*

   >[!NOTE]
   >  Windows, y compris PowerShell, calcule à l'aide de nombres binaires (base-2), tandis que les lecteurs sont souvent étiquetés à l’aide de nombres décimaux (base-10). Cela explique pourquoi un lecteur d'une capacité égale à « un téraoctet » (soit 1 000 000 000 000 octets) affiche une capacité d'environ « 909 Go » dans Windows. Ce comportement est normal. Lors de la création de volumes à l’aide de **New-Volume**, vous devez spécifier le paramètre **Size** à l'aide de nombres binaires (base-2). Par exemple, si vous spécifiez « 909 Go » ou « 0,909495 To », le volume créé sera d'environ 1 000 000 000 000 octets.

### <a name="example-with-2-or-3-servers"></a>Exemple : Avec les serveurs de 2 ou 3

Pour plus de simplicité, si votre déploiement possède uniquement deux serveurs, les espaces de stockage direct utiliseront automatiquement une mise en miroir double pour la résilience. Si votre déploiement possède uniquement trois serveurs, ils utiliseront automatiquement la mise en miroir triple.

```
New-Volume -FriendlyName "Volume1" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB
```

### <a name="example-with-4-servers"></a>Exemple : Avec 4 + serveurs

Si vous possédez quatre serveurs ou plus, vous pouvez utiliser le paramètre facultatif **ResiliencySettingName** pour choisir votre type de résilience.

-   **ResiliencySettingName:** Soit **miroir** ou **parité**.

Dans l’exemple suivant, *« Volume2 »* utilise la mise en miroir triple et *« Volume3 »* utilise la double parité (souvent appelée « codage d’effacement »).

```
New-Volume -FriendlyName "Volume2" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Mirror
New-Volume -FriendlyName "Volume3" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -Size 1TB -ResiliencySettingName Parity
```

### <a name="example-using-storage-tiers"></a>Exemple : À l’aide des niveaux de stockage

Dans les déploiements qui font appel à trois types de lecteurs, un volume peut s’étendre au niveau des disques SSD et du disque dur afin de résider en partie sur chacun d'entre eux. De la même façon, dans les déploiements faisant appel à quatre serveurs ou plus, un volume peut combiner la mise en miroir et la double parité afin de résider en partie sur chacun d'entre eux.

Pour vous aider à créer des volumes de ce type, les espaces de stockage direct fournissent des modèles de niveau par défaut appelés *Performances* et *Capacité*. Ils encapsulent les définitions pour la mise en miroir triple sur les lecteurs à capacité plus rapide (le cas échéant) et pour la double parité sur les lecteurs à capacité plus lente (le cas échéant).

Vous pouvez les afficher en exécutant l'applet de commande **Get-StorageTier**.

```
Get-StorageTier | Select FriendlyName, ResiliencySettingName, PhysicalDiskRedundancy
```

![Capture d'écran PowerShell des niveaux de stockage](media/creating-volumes/storage-tiers-screenshot.png)

Pour créer des volumes à plusieurs niveaux de stockage, référencez ces modèles de niveau à l'aide des paramètres **StorageTierFriendlyNames** et **StorageTierSizes** de l'applet de commande **New-Volume**. Par exemple, l’applet de commande suivant crée un volume qui mélange la mise en miroir triple et la double parité dans des proportions 30 : 70.

```
New-Volume -FriendlyName "Volume4" -FileSystem CSVFS_ReFS -StoragePoolFriendlyName S2D* -StorageTierFriendlyNames Performance, Capacity -StorageTierSizes 300GB, 700GB
```

## <a name="create-volumes-using-failover-cluster-manager"></a>Créer des volumes à l’aide du Gestionnaire du cluster de basculement

Vous pouvez également créer des volumes à l’aide de l'*Assistant Nouveau disque virtuel (espaces de stockage direct)*, suivi de l'*Assistant Nouveau volume* à partir du Gestionnaire du cluster de basculement, bien que ce flux de travail implique de nombreuses autres étapes manuelles et n’est pas recommandé.

Ce processus comprend trois étapes principales :

### <a name="step-1-create-virtual-disk"></a>Étape 1 : Créer le disque virtuel

![Nouveau disque virtuel](media/creating-volumes/GUI-Step-1.png)

1. Dans le Gestionnaire du cluster de basculement, accédez à **Stockage** -> **Pools**.
2. Sélectionnez **Nouveau disque virtuel** dans le volet Actions sur la droite, ou cliquez avec le bouton droit de la souris sur le pool, puis cliquez sur **Nouveau disque virtuel**.
3. Sélectionnez le pool de stockage et cliquez sur **OK**. L'*Assistant Nouveau disque virtuel (espaces de stockage direct)* s’ouvre.
4. Utilisez l’assistant pour renommer le disque virtuel et spécifier sa taille.
5. Passez en revue vos sélections et cliquez sur **Créer**.
6. Assurez-vous de cocher la case **Créer un volume lors de la fermeture de l’Assistant** avant la fermeture.

### <a name="step-2-create-volume"></a>Étape 2 : Créer un volume

L'*Assistant Nouveau volume* va s'ouvrir.

7. Sélectionnez le disque virtuel que vous venez de créer, puis cliquez sur **Suivant**.
8. Spécifiez la taille du volume (par défaut, sa taille est identique à celle du disque virtuel), puis cliquez sur **Suivant**. 
9. Affectez le volume à la lettre d'un lecteur ou choisissez **Ne pas affecter à la lettre d'un lecteur**, puis cliquez sur **Suivant**.
10. Spécifiez le système de fichiers à utiliser, laissez la taille d’unité d’allocation sur *Par défaut*, nommez le volume, puis cliquez sur **Suivant**.
11. Passez en revue vos sélections et cliquez sur **Créer**, puis **Fermer**.

### <a name="step-3-add-to-cluster-shared-volumes"></a>Étape 3 : Ajouter aux volumes partagés de cluster

![Ajouter aux volumes partagés de cluster](media/creating-volumes/GUI-Step-2.png)

12. Dans le Gestionnaire du cluster de basculement, accédez à **Stockage** -> **Disques**.
13. Sélectionnez le disque virtuel que vous venez de créer et sélectionnez **Ajouter aux volumes partagés de cluster** dans le volet Actions sur la droite, ou cliquez avec le bouton droit de la souris sur le disque virtuel, puis cliquez sur **Ajouter aux volumes partagés de cluster**.

C’est terminé ! Si nécessaire, répétez la procédure pour créer plusieurs volumes.

## <a name="see-also"></a>Voir aussi

- [Vue d’ensemble Direct des espaces de stockage](storage-spaces-direct-overview.md)
- [Planification des volumes dans les espaces de stockage Direct](plan-volumes.md)

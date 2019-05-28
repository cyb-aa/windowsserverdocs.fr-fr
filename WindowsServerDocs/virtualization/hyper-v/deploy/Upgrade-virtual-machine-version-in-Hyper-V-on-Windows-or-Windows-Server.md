---
title: Mise à niveau de version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server
description: Donne des instructions et des considérations relatives à la mise à niveau la version d’une machine virtuelle
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 1d19b3dc7000a4bf5558f351ce67ce7406b3d5d8
ms.sourcegitcommit: b190fac4bfa5599751a60d3fc3b4c4a64dd9afd7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/22/2019
ms.locfileid: "66009076"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Mise à niveau de version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server

>S'applique à : Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Proposer des dernières fonctionnalités Hyper-V sur vos machines virtuelles à la mise à niveau la version de configuration. Ne le faites pas jusqu'à ce que :

- Vous mettez à niveau vos hôtes Hyper-V vers la dernière version de Windows ou Windows Server.
- Vous mettez à niveau le niveau fonctionnel du cluster.
- Vous êtes sûr que vous n’aurez pas à ramener l’ordinateur virtuel à un hôte Hyper-V qui exécute une version antérieure de Windows ou Windows Server.

Pour plus d’informations, consultez [niveau propagée de Cluster système d’exploitation](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) et [effectuer une mise à niveau propagée d’un cluster hôte Hyper-V dans VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Étape 1 : Vérifier les versions de configuration de machine virtuelle

1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.
2. Avec le bouton droit de Windows PowerShell et sélectionnez **exécuter en tant qu’administrateur**.
3. Utilisez le [Get-VM](https://docs.microsoft.com/powershell/module/hyper-v/get-vm)applet de commande. Exécutez la commande suivante pour obtenir les versions de vos machines virtuelles.

```PowerShell
Get-VM * | Format-Table Name, Version
```

Vous pouvez également voir la version de configuration dans le Gestionnaire Hyper-V en sélectionnant la machine virtuelle et en examinant le **Résumé** onglet.

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Étape 2 : Mise à niveau la version de configuration de machine virtuelle

1. Arrêter la machine virtuelle dans le Gestionnaire Hyper-V.
2. Sélectionnez Action > mettre à niveau la Version de Configuration. Si cette option n’est pas disponible pour la machine virtuelle, puis il est déjà la version la plus récente de la configuration prise en charge par l’hôte Hyper-V.

Pour mettre à niveau la version de configuration de machine virtuelle à l’aide de Windows PowerShell, utilisez le [mise à jour-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) applet de commande. Exécutez la commande suivante où vmname est le nom de la machine virtuelle.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="BKMK_SupportedConfigVersions"></a>Versions de configuration de machine virtuelle prises en charge

Exécutez l’applet de commande PowerShell [Get-VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) pour voir quelles versions de configuration de machine virtuelle prend en charge de votre hôte Hyper-V. Lorsque vous créez une machine virtuelle, il est créé avec la version de configuration par défaut. Pour découvrir les nouveautés de la valeur par défaut, exécutez la commande suivante.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Si vous avez besoin créer une machine virtuelle que vous pouvez déplacer vers un ordinateur hôte Hyper-V qui exécute une version antérieure de Windows, utilisez le [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) applet de commande avec le paramètre - version. Par exemple, pour créer une machine virtuelle que vous pouvez déplacer vers un ordinateur hôte Hyper-V qui exécute Windows Server 2012 R2, exécutez la commande suivante. Cette commande crée un ordinateur virtuel nommé « WindowsCV5 » avec une version de configuration 5.0.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Vous pouvez importer les ordinateurs virtuels qui ont été créés pour un hôte Hyper-V exécutant une version antérieure de Windows ou les restaurer à partir de la sauvegarde. Si la version de configuration de la machine virtuelle n’est pas répertoriée comme pris en charge pour votre système d’exploitation de l’hôte de Hyper-V dans le tableau ci-dessous, vous devez mettre à jour la version de configuration de machine virtuelle avant de commencer la machine virtuelle.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versions de configuration de machine virtuelle prises en charge pour les hôtes de maintenance à long terme

Le tableau suivant répertorie les versions de configuration de machine virtuelle sont prises en charge sur les ordinateurs hôtes exécutant une version de maintenance à long terme de Windows.

| Version de Windows hôte Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 entreprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Entreprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Enterprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versions de configuration de machine virtuelle prises en charge pour les hôtes de canal semi-annuel

Le tableau suivant répertorie les versions de configuration de machine virtuelle pour les ordinateurs hôtes exécutant une version de canal semestriel actuellement prises en charge de Windows. Pour obtenir plus d’informations sur les versions de canal semestriel de Windows, visitez les pages suivantes pour [Windows Server](../../../get-started-19/servicing-channels-19.md) et [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Version de Windows hôte Hyper-V | 9.1 | 9.0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6.2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Windows 10 peut 2019 mettre à jour (version 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, version 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, version 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 octobre 2018 Update (version 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, version 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|10 avril 2018 de Windows Update (version 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Fall Creators Update (version 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (version 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|À jour anniversaire Windows 10 (version 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Pourquoi dois-je mettre à niveau la version de configuration de machine virtuelle ?

Quand vous déplacez ou importez un ordinateur virtuel sur un ordinateur qui exécute Hyper-V sur Windows Server 2019, Windows Server 2016 ou Windows 10, la machine virtuelle « configuration de s n’est pas automatiquement mis à jour. Cela signifie que vous pouvez revenir l’ordinateur virtuel à un hôte Hyper-V qui exécute une version antérieure de Windows ou Windows Server. Toutefois, cela signifie également que vous ne pouvez pas utiliser certaines des nouvelles fonctionnalités de machine virtuelle jusqu'à ce que vous mettez à jour la version de configuration manuellement. Vous ne pouvez pas rétrograder la version de configuration de machine virtuelle une fois que vous avez mis à niveau.

La version de configuration de machine virtuelle représente la compatibilité de la configuration de la machine virtuelle, enregistré l’état et les fichiers de capture instantanée avec la version d’Hyper-V. Lorsque vous mettez à jour la version de configuration, vous modifiez la structure de fichier qui est utilisée pour stocker la configuration de machines virtuelles et les fichiers de point de contrôle. Vous mettez également à jour la version de configuration vers la dernière version prise en charge par cet hôte Hyper-V. Les machines virtuelles mises à niveau utilisent un nouveau format de fichier de configuration qui est conçu pour accroître les performances de lecture et d’écriture des données de configuration de machine virtuelle. La mise à niveau permet également de réduire le risque de corruption des données en cas de défaillance du stockage.

Le tableau suivant répertorie les descriptions, les extensions de nom de fichier et les emplacements par défaut pour chaque type de fichier qui est utilisé pour les ordinateurs virtuels nouveaux ou mis à niveau.

 |Types de fichiers de machine virtuelle | Description|
 |---|---|
|Configuration |Informations de configuration de machine virtuelle qui sont stockées dans un format de fichier binaire. <br /> Extension de nom de fichier : .vmcx <br /> Emplacement par défaut : C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
 |État d’exécution|Machine virtuelle informations d’état runtime qui sont stockées dans un format de fichier binaire. <br />Extension de nom de fichier : .vmrs et .vmgs <br />Emplacement par défaut : C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual Machines|
|Disque dur virtuel|Stocke les disques durs virtuels pour la machine virtuelle. <br /> Extension de nom de fichier : .vhd ou .vhdx <br />Emplacement par défaut : Disques durs C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Disque dur virtuel automatiques |Fichiers disque de différenciation utilisés pour les points de contrôle de machine virtuelle. <br /> Extension de nom de fichier : .avhdx <br /> Emplacement par défaut : Disques durs C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Point de contrôle|Les points de contrôle sont stockés dans plusieurs fichiers de point de contrôle. Chaque point de contrôle crée un fichier de configuration et un fichier d’état d’exécution. <br /> Extensions de nom de fichier : .vmrs et .vmcx <br />Emplacement par défaut : C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>Que se passe-t-il si je ne mettez à niveau la version de configuration de machine virtuelle ?

Si vous avez des machines virtuelles que vous avez créé avec une version antérieure d’Hyper-V, certaines fonctionnalités qui sont disponibles sur l’hôte plus récente du que système d’exploitation peut ne pas fonctionne avec ces ordinateurs virtuels jusqu'à ce que vous mettez à jour la version de configuration.

Comme une aide générale, nous vous recommandons la mise à jour de la version de configuration une fois que vous avez correctement mis à niveau vers une version plus récente de Windows, les hôtes de virtualisation et de grandes chances que vous n’avez pas besoin de restaurer. Lorsque vous utilisez le [système d’exploitation de la mise à niveau propagée de cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) fonctionnalité, il s’agit en général, après la mise à jour le niveau fonctionnel du cluster. De cette façon, vous bénéficierez de nouvelles fonctionnalités et les changements internes et les optimisations également.

>[!NOTE]
>Une fois que la version de configuration de machine virtuelle est mise à jour, la machine virtuelle ne pourrez pas démarrer sur des hôtes qui ne prennent pas en charge la version de configuration mis à jour.

Le tableau suivant présente la version de configuration des machines virtuelles minimum requise pour utiliser certaines fonctionnalités Hyper-V.

|Fonctionnalité|Version de configuration de machine virtuelle minimale|
|---|---|
|Ajout/suppression de mémoire à chaud|6.2|
|Démarrage sécurisé pour les machines virtuelles Linux|6.2|
|Points de contrôle de production|6.2|
|PowerShell Direct |6.2|
|Regroupement de machines virtuelles|6.2|
|Module de plateforme sécurisée virtuelle (vTPM)|7.0|
|Files d’attente de plusieurs machines virtuelles (VMMQ)|7.1|
|Prise en charge XSAVE|8.0|
|Lecteur de stockage de clés|8.0|
|Prise en charge de la sécurité basée sur la virtualisation invité (VBS)|8.0|
|Virtualisation imbriquée|8.0|
|Nombre de processeurs virtuels|8.0|
|Mémoire de grande taille des machines virtuelles|8.0|
|Augmenter le nombre maximal par défaut pour les périphériques virtuels à 64 par appareil (par exemple, mise en réseau et attribués les appareils)|8.3|
|Autoriser les fonctionnalités de processeur supplémentaires pour l’Analyseur de performances|9.0|
|Exposer automatiquement [simultanées multithreading](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) configuration pour les machines virtuelles en cours d’exécution sur des ordinateurs hôtes à l’aide de la [Core planificateur](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9.0|
|Prise en charge de la mise en veille prolongée|9.0|

Pour plus d’informations sur ces fonctionnalités, consultez [quelles sont les nouveautés dans Hyper-V sur Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).


---
title: Mettre à niveau la version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server
description: Fournit des instructions et des considérations relatives à la mise à niveau de la version d’une machine virtuelle
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 897f2454-5aee-445c-a63e-f386f514a0f6
author: jasongerend
ms.author: jgerend
ms.date: 05/22/2019
ms.openlocfilehash: 96678dfab2a3d5b6f503d8ce9d00850a3c437b35
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78370603"
---
# <a name="upgrade-virtual-machine-version-in-hyper-v-on-windows-10-or-windows-server"></a>Mettre à niveau la version de la machine virtuelle dans Hyper-V sur Windows 10 ou Windows Server

>S’applique à : Windows 10, Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Rendez les fonctionnalités Hyper-V les plus récentes disponibles sur vos machines virtuelles en effectuant une mise à niveau de la version de configuration. N’effectuez pas cette opération tant que :

- Vous mettez à niveau vos ordinateurs hôtes Hyper-V vers la dernière version de Windows ou Windows Server.
- Vous mettez à niveau le niveau fonctionnel du cluster.
- Vous êtes sûr de ne pas avoir à déplacer la machine virtuelle vers un ordinateur hôte Hyper-V qui exécute une version antérieure de Windows ou Windows Server.

Pour plus d’informations, consultez [mise à niveau propagée de système d’exploitation de cluster](../../../failover-clustering/Cluster-Operating-System-Rolling-Upgrade.md) et [effectuer une mise à niveau propagée d’un cluster hôte Hyper-V dans VMM](https://docs.microsoft.com/system-center/vmm/hyper-v-rolling-upgrade).

## <a name="step-1-check-the-virtual-machine-configuration-versions"></a>Étape 1 : vérifier les versions de configuration des machines virtuelles

1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.
2. Cliquez avec le bouton droit sur Windows PowerShell, puis sélectionnez **exécuter en tant qu’administrateur**.
3. Utilisez l’applet de commande [« obtient-VM »](https://docs.microsoft.com/powershell/module/hyper-v/get-vm). Exécutez la commande suivante pour récupérer les versions de vos machines virtuelles.

```PowerShell
Get-VM * | Format-Table Name, Version
```

Vous pouvez également voir la version de configuration dans le Gestionnaire Hyper-V en sélectionnant la machine virtuelle et en examinant l’onglet **Résumé** .

## <a name="step-2-upgrade-the-virtual-machine-configuration-version"></a>Étape 2 : mettre à niveau la version de la configuration de la machine virtuelle

1. Arrêtez l’ordinateur virtuel dans le Gestionnaire Hyper-V.
2. Sélectionnez action > mettre à niveau la version de configuration. Si cette option n’est pas disponible pour la machine virtuelle, il s’agit déjà de la version de configuration la plus élevée prise en charge par l’hôte Hyper-V.

Pour mettre à niveau la version de configuration de la machine virtuelle à l’aide de Windows PowerShell, utilisez l’applet de commande [Update-VMVersion](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) . Exécutez la commande suivante, où vmname est le nom de l’ordinateur virtuel.

```PowerShell
Update-VMVersion <vmname>
```

## <a name="supported-virtual-machine-configuration-versions"></a>Versions de configuration d’ordinateur virtuel prises en charge

Exécutez l’applet de commande PowerShell [VMHostSupportedVersion](https://docs.microsoft.com/powershell/module/hyper-v/get-vmhostsupportedversion) pour voir quelles sont les versions de configuration d’ordinateur virtuel prises en charge par votre hôte Hyper-V. Lorsque vous créez un ordinateur virtuel, il est créé avec la version de configuration par défaut. Pour voir la valeur par défaut, exécutez la commande suivante.

```PowerShell
Get-VMHostSupportedVersion -Default
```

Si vous avez besoin de créer une machine virtuelle que vous pouvez déplacer vers un ordinateur hôte Hyper-V qui exécute une version antérieure de Windows, utilisez l’applet de commande [New-VM](https://docs.microsoft.com/powershell/module/hyper-v/new-vm) avec le paramètre-version. Par exemple, pour créer un ordinateur virtuel que vous pouvez déplacer vers un hôte Hyper-V qui exécute Windows Server 2012 R2, exécutez la commande suivante. Cette commande crée un ordinateur virtuel nommé « WindowsCV5 » avec une version de configuration 5,0.

```PowerShell
New-VM -Name "WindowsCV5" -Version 5.0
```

>[!NOTE]
>Vous pouvez importer des ordinateurs virtuels qui ont été créés pour un ordinateur hôte Hyper-V exécutant une version antérieure de Windows ou les restaurer à partir de la sauvegarde. Si la version de configuration de la machine virtuelle n’est pas indiquée comme prise en charge pour votre système d’exploitation hôte Hyper-V dans le tableau ci-dessous, vous devez mettre à jour la version de configuration de machine virtuelle avant de pouvoir démarrer la machine virtuelle.

### <a name="supported-vm-configuration-versions-for-long-term-servicing-hosts"></a>Versions de configuration d’ordinateur virtuel prises en charge pour les hôtes de maintenance à long terme

Le tableau suivant répertorie les versions de configuration de machine virtuelle qui sont prises en charge sur les ordinateurs hôtes exécutant une version de maintenance à long terme de Windows.

| Version de Windows de l’hôte Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6,2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
|Windows Server 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Entreprise LTSC 2019|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server 2016|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Entreprise 2016 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Entreprise 2015 LTSB|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|
|Windows Server 2012 R2|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|
|Windows 8.1|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|

### <a name="supported-vm-configuration-versions-for-semi-annual-channel-hosts"></a>Versions de configuration d’ordinateur virtuel prises en charge pour les hôtes de canal semi-annuel

Le tableau suivant répertorie les versions de configuration de machine virtuelle pour les hôtes qui exécutent une version de Windows Channel semi-annuelle actuellement prise en charge. Pour obtenir plus d’informations sur les versions de canal semi-annuelles de Windows, visitez les pages suivantes pour [Windows Server](../../../get-started-19/servicing-channels-19.md) et [Windows 10](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels)

| Version de Windows de l’hôte Hyper-V | 9,1 | 9,0 | 8.3 | 8.2 | 8.1 | 8.0 | 7.1 | 7.0 | 6,2 | 5.0 |
| --- |---|---|---|---|---|---|---|---|---|---|
| Mise à jour de Windows 10 mai 2019 (version 1903) |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
| Windows Server, version 1903 |&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;| &#10004;|
|Windows Server, version 1809|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mise à jour 2018 de Windows 10 octobre (version 1809)|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows Server, version 1803|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mise à jour 2018 de Windows 10 avril (version 1803)|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mise à jour des créateurs de automne Windows 10 (version 1709)|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Windows 10 Creators Update (version 1703)|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|
|Mise à jour anniversaire Windows 10 (version 1607)|&#10006;|&#10006;|&#10006;|&#10006;|&#10006;|&#10004;|&#10004;|&#10004;|&#10004;|&#10004;|

## <a name="why-should-i-upgrade-the-virtual-machine-configuration-version"></a>Pourquoi dois-je mettre à niveau la version de la configuration de la machine virtuelle ?

Lorsque vous déplacez ou importez un ordinateur virtuel sur un ordinateur exécutant Hyper-V sur Windows Server 2019, Windows Server 2016 ou Windows 10, la configuration de l’ordinateur virtuel n’est pas automatiquement mise à jour. Cela signifie que vous pouvez déplacer la machine virtuelle vers un ordinateur hôte Hyper-V qui exécute une version précédente de Windows ou de Windows Server. Mais cela signifie également que vous ne pouvez pas utiliser les nouvelles fonctionnalités de la machine virtuelle tant que vous n’avez pas mis à jour manuellement la version de configuration. Vous ne pouvez pas rétrograder la version de la configuration de la machine virtuelle une fois que vous l’avez mise à niveau.

La version de configuration de l’ordinateur virtuel représente la compatibilité de la configuration de l’ordinateur virtuel, de l’état enregistré et des fichiers d’instantanés avec la version d’Hyper-V. Lorsque vous mettez à jour la version de configuration, vous modifiez la structure de fichiers utilisée pour stocker la configuration des machines virtuelles et les fichiers de point de contrôle. Vous devez également mettre à jour la version de configuration vers la dernière version prise en charge par cet hôte Hyper-V. Les machines virtuelles mises à niveau utilisent un nouveau format de fichier de configuration qui est conçu pour accroître les performances de lecture et d’écriture des données de configuration de machine virtuelle. La mise à niveau permet également de réduire le risque de corruption des données en cas de défaillance du stockage.

Le tableau suivant répertorie les descriptions, les extensions de nom de fichier et les emplacements par défaut pour chaque type de fichier utilisé pour les machines virtuelles nouvelles ou mises à niveau.

 |Types de fichiers de machine virtuelle | Description|
 |---|---|
|Configuration |Informations de configuration de l’ordinateur virtuel stockées au format de fichier binaire. <br /> Extension de nom de fichier :. vmcx <br /> Emplacement par défaut : machines C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |État d’exécution|Informations d’état d’exécution de machine virtuelle stockées au format de fichier binaire. <br />Extension de nom de fichier :. VMRS et. vmgs <br />Emplacement par défaut : machines C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
|Disque dur virtuel|Stocke les disques durs virtuels de la machine virtuelle. <br /> Extension de nom de fichier :. vhd ou. vhdx <br />Emplacement par défaut : disques durs C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Disque dur virtuel automatique |Fichiers de disque de différenciation utilisés pour les points de contrôle de la machine virtuelle. <br /> Extension de nom de fichier :. avhdx <br /> Emplacement par défaut : disques durs C:\ProgramData\Microsoft\Windows\Hyper-V\Virtual|
 |Point de contrôle|Les points de contrôle sont stockés dans plusieurs fichiers de point de contrôle. Chaque point de contrôle crée un fichier de configuration et un fichier d’état d’exécution. <br /> Extensions de nom de fichier :. VMRS et. vmcx <br />Emplacement par défaut : C:\ProgramData\Microsoft\Windows\Snapshots|

## <a name="what-happens-if-i-dont-upgrade-the-virtual-machine-configuration-version"></a>Que se passe-t-il si je ne parvient pas à mettre à niveau la version de la configuration

Si vous avez créé des machines virtuelles avec une version antérieure d’Hyper-V, certaines fonctionnalités disponibles sur le système d’exploitation hôte plus récent peuvent ne pas fonctionner avec ces machines virtuelles tant que vous n’avez pas mis à jour la version de configuration.

En guise d’aide générale, nous vous recommandons de mettre à jour la version de configuration une fois que vous avez correctement mis à niveau les hôtes de virtualisation vers une version plus récente de Windows et que vous n’avez pas besoin de procéder à une restauration. Lorsque vous utilisez la fonctionnalité de [mise à niveau propagée du système d’exploitation du cluster](https://docs.microsoft.com/windows-server/failover-clustering/Cluster-Operating-System-Rolling-Upgrade) , cela se produit généralement après la mise à jour du niveau fonctionnel du cluster. De cette façon, vous bénéficiez également de nouvelles fonctionnalités, ainsi que d’optimisations et de modifications internes.

>[!NOTE]
>Une fois la version de configuration de machine virtuelle mise à jour, la machine virtuelle ne peut pas démarrer sur les ordinateurs hôtes qui ne prennent pas en charge la version de configuration mise à jour.

Le tableau suivant indique la version minimale de configuration de machine virtuelle requise pour l’utilisation de certaines fonctionnalités Hyper-V.

|Composant|Version minimale de configuration de machine virtuelle|
|---|---|
|Ajout/suppression de mémoire à chaud|6,2|
|Démarrage sécurisé pour les machines virtuelles Linux|6,2|
|Points de contrôle de production|6,2|
|PowerShell Direct |6,2|
|Regroupement de machines virtuelles|6,2|
|Module de plateforme sécurisée virtuelle (vTPM)|7.0|
|Plusieurs files d’attente d’ordinateurs virtuels (VMMQ)|7.1|
|Support XSAVE|8.0|
|Lecteur de stockage de clés|8.0|
|Prise en charge de la sécurité basée sur la virtualisation invité (VBS)|8.0|
|Virtualisation imbriquée|8.0|
|Nombre de processeurs virtuels|8.0|
|Machines virtuelles à mémoire élevée|8.0|
|Augmenter le nombre maximal par défaut d’appareils virtuels sur 64 par appareil (par exemple, mise en réseau et appareils attribués)|8.3|
|Autoriser des fonctionnalités de processeur supplémentaires pour Perfmon|9,0|
|Exposer automatiquement la configuration [multithread simultanée](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#background) pour les machines virtuelles exécutées sur des ordinateurs hôtes à l’aide du [planificateur Core](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types#windows-server-2019-hyper-v-defaults-to-using-the-core-scheduler)|9,0|
|Prise en charge de la mise en veille prolongée|9,0|

Pour plus d’informations sur ces fonctionnalités, consultez [Nouveautés d’Hyper-V sur Windows Server](../What-s-new-in-Hyper-V-on-Windows.md).


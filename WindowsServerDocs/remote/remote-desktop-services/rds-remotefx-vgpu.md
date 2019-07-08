---
title: Services Bureau à distance - Installation et configuration d'un processeur graphique virtuel (vGPU) RemoteFX
description: Informations de planification pour la configuration de la virtualisation graphique du processeur graphique virtuel (vGPU) RemoteFX.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 03/23/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0263fa6b-2185-4cc3-99ef-3588e2f4ada5
author: lizap
manager: scottman
ms.openlocfilehash: 3e7da1a70826dc720a96ceb3fe5d04868943f163
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63712138"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Installer et configurer un processeur graphique virtuel (vGPU) RemoteFX pour les services Bureau à distance


La fonctionnalité vGPU de RemoteFX permet à plusieurs machines virtuelles de partager une carte graphique physique. Les machines virtuelles peuvent décharger le rendu des informations graphiques du processeur vers la carte graphique dédiée. Cela permet de réduire la charge au niveau du processeur et d'améliorer l'évolutivité pour les charges de travail graphiques intenses exécutées sur les machines virtuelles VDI. 

## <a name="remotefx-vgpu-requirements"></a>Configuration requise pour la fonctionnalité vGPU de RemoteFX

Configuration requise pour les systèmes hôtes : 

- Windows Server 2016 ou Windows 10
- GPU compatible DX 11.0 avec pilote compatible WDDM 1.2 
- Rôle Hôte de virtualisation des services Bureau à distance de Windows Server activé (active le rôle Hyper-V) 
- Serveur doté d'un processeur prenant en charge la fonctionnalité SLAT (Second Level Address Translation). 

Configuration requise pour la machine virtuelle invitée :

- Machine virtuelle invitée exécutant un client Windows Entreprise (Windows 7 avec Service Pack 1, Windows 8.1, Windows 10) ou Windows Server (Windows Server 2012 R2 ou Windows Server 2016). Pour plus d'informations sur la prise en charge, consultez [Configuration prise en charge pour les services Bureau à distance](rds-supported-config.md).

Autres considérations relatives aux machines virtuelles invitées :

- Les fonctionnalités OpenGL et OpenCL sont uniquement disponibles sous Windows 10 et Windows Server 2016.  
- DirectX 11.0 est uniquement disponible avec les machines virtuelles invitées Windows 8 ou versions ultérieures. 
- L'hôte de session Bureau à distance n'est pris en charge par la fonctionnalité vGPU de RemoteFX que s'il est exécuté en tant que [bureau de session personnel](rds-personal-session-desktops.md).

Pour les machines virtuelles invitées, consultez [Déploiement VDI - Systèmes d'exploitation invités pris en charge](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Installer la fonctionnalité vGPU de RemoteFX

Suivez les étapes ci-dessous pour installer et configurer RemoteFX sur l'hôte pour Windows Server 2016 et Windows 10 :

1. Installez le système d’exploitation.
2. Installez les derniers pilotes GPU Windows 10/Windows Server 2016 disponibles sur le site du fournisseur de la carte graphique.
3. Installez la fonctionnalité vGPU de RemoteFX sur l'hôte Windows 10/Windows Server 2016 :
   1. Sur un hôte Windows 10, activez la fonctionnalité Hyper-V à partir du Panneau de configuration (accédez à Panneau de configuration/Programmes et fonctionnalités /Activer ou désactiver des fonctionnalités Windows) :

      ![Fenêtre Fonctionnalités Windows pour activer la fonctionnalité Hyper-V](media/rds-hyperv-settings.png)

   2. Sur un hôte Windows Server 2016, installez le rôle Serveur hôte de virtualisation des services Bureau à distance.
   

4. À présent, créez et configurez une machine virtuelle invitée :
   1. Créez une machine virtuelle avec Windows 10 Entreprise ou Windows Server 2016.
   2. Ajoutez la carte graphique RemoteFX 3D. Consultez [Configurer la carte RemoteFX vGPU 3D](#configure-the-remotefx-vgpu-3d-adapter) pour en savoir plus sur l'utilisation du Gestionnaire Hyper-V ou des applets de commande PowerShell. 

La fonctionnalité vGPU de RemoteFX utilisera tous les GPU lorsqu'il en existe plusieurs. Toutefois, dans certains cas, vous pouvez limiter les GPU utilisés par RemoteFX. Dans l'environnement Hyper-V, vous pouvez sélectionner spécifiquement les GPU qui ne doivent *pas* être utilisés par RemoteFX. Effectuez les étapes suivantes : 

   1. Dans le Gestionnaire Hyper-V, accédez aux Paramètres Hyper-V.
   2. Dans Paramètres Hyper-V, cliquez sur **GPU physiques**.
   3. Sélectionnez le GPU que vous ne souhaitez pas utiliser, puis décochez **Utiliser cette unité GPU avec RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurer la carte RemoteFX vGPU 3D
Vous pouvez configurer la carte graphique RemoteFX vGPU 3D à l'aide de l'interface utilisateur du Gestionnaire Hyper-V ou des applets de commande PowerShell. 

#### <a name="through-hyper-v-manager"></a>À l'aide du Gestionnaire Hyper-V :

1. Assurez-vous que le système a été configuré avec Hyper-V et qu'une machine virtuelle est configurée.  
2. Arrêtez la machine virtuelle si elle est en cours d'exécution. 
3. Dans le Gestionnaire Hyper-V, accédez à **Paramètres de la machine virtuelle**, puis cliquez sur **Ajout de matériel**.
4. Sélectionnez **Carte graphique RemoteFX 3D**, puis cliquez sur **Ajouter**. 
5. Définissez le nombre maximum de moniteurs, la résolution maximale de ceux-ci et la mémoire vidéo dédiée, ou conservez les valeurs par défaut.

   > [!NOTE]
   > - La définition de valeurs plus élevées pour l'une ou l'autre de ces options aura un impact sur la mise à l'échelle. Vous ne devez donc définir que ce qui est absolument nécessaire.
   >
   > - Si vous avez besoin de 1 Go de VRAM dédiée, utilisez une machine virtuelle invitée de 64 bits au lieu de 32 bits (x86) pour de meilleurs résultats.
6. Cliquez sur **OK** pour terminer la configuration.

#### <a name="with-powershell-cmdlets"></a>À l'aide des applets de commande PowerShell :

Exécutez les applets de commande suivantes pour ajouter, passer en revue et configurer la carte : 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Pour plus d'informations, consultez [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Pour plus d'informations, consultez [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Pour plus d'informations, consultez [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Exécutez l'applet de commande suivante pour passer en revue les GPU physiques :

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Pour plus d'informations, consultez [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Analyser les performances

Les performances et la mise à l'échelle d'un système VDI dépendent de divers facteurs tels que la mémoire totale du GPU, la quantité de mémoire système et sa vitesse, le nombre de cœurs du processeur et la fréquence d'horloge de l'UC, la vitesse de stockage et l'implémentation de l'architecture NUMA.

La prise en charge de la fonctionnalité vGPU à distance dans les services Bureau à distance inclut les compteurs de performance suivants, que vous pouvez consulter dans Analyseur de performances (PerfMon.exe) afin de recueillir des informations sur la fréquence d'images.

- Vidéo RemoteFX : compteurs destinés à la compression vidéo du protocole RDP (Remote Desktop Protocol). Par exemple, si vous souhaitez connaître le nombre d'images présentées à RDP pour la compression, consultez le compteur **Images d'entrée/seconde**.
- Réseau RemoteFX : compteurs destinés au trafic réseau du protocole RDP (Remote Desktop Protocol). Par exemple, **Durée des boucles**.
- Gestion des GPU racines RemoteFX : mesure la VRAM disponible et réservée.
- Logiciel RemoteFX : fournit des compteurs pour le taux de captures, le temps de réponse GPU, etc.

Pour une surveillance plus poussée des performances, en particulier pour la résolution des problèmes, vous pouvez utiliser les compteurs de performances supplémentaires suivants :

- Périphérique de machine virtuelle VSC Synth3D RemoteFX 
- Canal de transport de machine virtuelle VSC Synth3D RemoteFX 
- VSC Synth3D RemoteFX 
- Périphérique de machine virtuelle VSP Synth3D RemoteFX 
- Canal de transport de machine virtuelle VSP Synth3D RemoteFX

---
title: Services Bureau à distance - RemoteFX vGPU installation et configuration
description: Informations de planification pour configurer la virtualisation de graphiques de vGPU RemoteFX.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876840"
---
# <a name="set-up-and-configure-remotefx-vgpu-for-remote-desktop-services"></a>Installer et configurer RemoteFX vGPU pour les Services Bureau à distance


La fonctionnalité de vGPU de RemoteFX rend possible pour plusieurs machines virtuelles de partager une carte graphique physique. Les machines virtuelles sont en mesure de décharger le rendu des informations graphiques à partir du processeur à la carte graphique dédiée. Cela diminue la charge du processeur et améliorer l’évolutivité pour les graphiques intenses les charges de travail qui s’exécutent dans les machines virtuelles VDI. 

## <a name="remotefx-vgpu-requirements"></a>Configuration requise du processeur graphique virtuel RemoteFX

Configuration requise pour les systèmes hôtes : 

- Windows Server 2016 ou Windows 10
- DX 11.0 de GPU compatible avec pilote WDDM 1.2 
- Rôle de l’hôte de virtualisation Bureau à distance de Windows Server activé (et Active le rôle Hyper-V) 
- Serveur avec un processeur qui prend en charge SLAT (Second Level Address Translation) 

Configuration requise des machines virtuelles invitées :

- Machines virtuelles invitées exécute un client d’entreprise de Windows (Windows 7 avec Service Pack 1, Windows 8.1, Windows 10) ou d’un serveur Windows (Windows Server 2012 R2 ou Windows Server 2016). Pour le système d’exploitation supplémentaires prennent en charge consultez [configuration pris en charge pour les Services Bureau à distance](rds-supported-config.md).

Considérations supplémentaires pour les machines virtuelles invitées :

- OpenGL et OpenCL fonctionnalité est uniquement disponible dans Windows 10 ou Windows Server 2016.  
- DirectX 11.0 est uniquement disponible avec Windows 8 ou les machines virtuelles invitées plus récente. 
- Hôte de Session Bureau à distance est uniquement pris en charge avec RemoteFX vGPU s’il s’exécute comme un [desktop de session personnelle](rds-personal-session-desktops.md).

Pour les invités d’ordinateurs virtuels, passez en revue [déploiement VDI - des systèmes d’exploitation invités pris en charge](rds-supported-config.md#vdi-deployment--supported-guest-oss).

## <a name="install-remotefx-vgpu"></a>Installer le processeur graphique virtuel RemoteFX

Utilisez les étapes suivantes pour installer et configurer RemoteFX sur l’ordinateur hôte pour Windows Server 2016 et Windows 10 :

1. Installez le système d’exploitation.
2. Installez les dernière Windows 10/Windows pilotes GPU Server 2016 disponibles à partir du site de fournisseur de carte graphique.
3. Installer le processeur graphique virtuel RemoteFX sur l’hôte de Windows 10/Windows Server 2016 :
   1. Sur un ordinateur hôte Windows 10, activer la fonctionnalité de Hyper-V dans le panneau de configuration (accédez à panneau de configuration/programmes et fonctionnalités de Windows de fonctionnalités/activer ou désactiver) :

      ![Fenêtre de fonctionnalités de Windows pour activer la fonctionnalité Hyper-V](media/rds-hyperv-settings.png)

   2. Sur un ordinateur hôte Windows Server 2016, installez le rôle de l’hôte de virtualisation de Bureau à distance (RDVH).
   

4. À présent, créer et configurer une machine virtuelle invitée :
   1. Créer une machine virtuelle avec Windows 10 entreprise ou Windows Server 2016.
   2. Ajoutez la carte de graphismes 3D RemoteFX. Consultez [configurer la carte RemoteFX vGPU 3D](#configure-the-remotefx-vgpu-3d-adapter) pour plus d’informations sur la façon de le faire avec les applets de commande Hyper-V Manager ou PowerShell. 

RemoteFX vGPU utilisera tous les processeurs graphiques lorsque plusieurs sont disponibles. Toutefois, dans certains cas, que vous souhaiterez peut-être limiter les GPU sont utilisées par RemoteFX. Dans l’environnement Hyper-V, vous contrôle ce comportement en sélectionnant spécifiquement le qui doivent GPU *pas* utilisable par RemoteFX. Effectuez les étapes suivantes : 

   1. Accédez aux paramètres Hyper-V dans le Gestionnaire Hyper-V.
   2. Cliquez sur **GPU physiques** dans les paramètres Hyper-V.
   3. Sélectionnez le GPU que vous ne souhaitez pas utiliser, puis désactivez **utilisent ce GPU avec RemoteFX**.


### <a name="configure-the-remotefx-vgpu-3d-adapter"></a>Configurer la carte RemoteFX vGPU 3D
Vous pouvez utiliser les applets de commande de l’interface utilisateur du Gestionnaire Hyper-V ou de PowerShell pour configurer l’adaptateur de graphismes 3D de vGPU RemoteFX. 

#### <a name="through-hyper-v-manager"></a>Via le Gestionnaire Hyper-V :

1. Vérifiez que le système a été configuré avec Hyper-V et a une machine virtuelle configurée.  
2. Arrêter la machine virtuelle, si elle est en cours d’exécution. 
3. Dans le Gestionnaire Hyper-V, accédez à la **paramètres de machine virtuelle**, puis cliquez sur **Ajout de matériel**.
4. Sélectionnez **les carte vidéo 3D RemoteFX**, puis cliquez sur **ajouter**. 
5. Définir le nombre maximal d’analyses, de résolution d’écran maximale et de mémoire vidéo dédiée, ou laissez les valeurs par défaut.

   > [!NOTE]
   > - Définition des valeurs supérieures pour chacune de ces options aura impact à l’échelle, donc vous ne devez définir ce qui est absolument nécessaire.
   >
   > - Lorsque vous avez besoin d’utiliser 1 Go de RAM vidéo dédiée, vous pouvez utiliser un ordinateur virtuel de l’invité 64 bits au lieu de 32 bits (x86) pour de meilleurs résultats.
6. Cliquez sur **OK** pour terminer la configuration.

#### <a name="with-powershell-cmdlets"></a>Avec les applets de commande PowerShell :

Exécutez les applets de commande suivantes pour ajouter, passez en revue et configurer l’adaptateur : 

```powershell
Add-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Pour plus d’informations, consultez [Add-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/add-vmremotefx3dvideoadapter).

```powershell
Get-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>]  [-Credential <PSCredential[]>] [-VMName] <String[]> [<CommonParameters>]
```

Pour plus d’informations, consultez [Get-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefx3dvideoadapter)

```powershell
Set-VMRemoteFx3dVideoAdapter [-CimSession <CimSession[]>] [-ComputerName <String[]>] [-Credential <PSCredential[]>] [-VMName] <String[]> [[-MonitorCount] <Byte>] [[-MaximumResolution] <String>] [[-VRAMSizeBytes] <UInt64>] [-Passthru] [-WhatIf] [-Confirm] [<CommonParameters>]
```

Pour plus d’informations, consultez [Set-VMRemoteFx3dVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/set-vmremotefx3dvideoadapter).

Exécutez l’applet de commande suivante pour passer en revue les GPU physiques :

```powershell
Get-VMRemoteFXPhysicalVideoAdapter [-ComputerName <String[]>] [-Credential <PSCredential[]>] [[-Name] <String[]>] [<CommonParameters>]  
```

Pour plus d’informations, consultez [Get-VMRemoteFXPhysicalVideoAdapter](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/get-vmremotefxphysicalvideoadapter).

## <a name="monitor-performance"></a>Surveiller les performances

Les performances et la mise à l’échelle d’un système VDI sont déterminées par une variété de facteurs tels que la mémoire totale du GPU, quantité de mémoire système et de vitesse de la mémoire, nombre de cœurs de processeur et fréquence d’horloge du processeur, la vitesse de stockage et implémentation de NUMA.

Prise en charge de vGPU distant dans les services Bureau à distance inclut les compteurs de performances suivants, vous pouvez afficher dans l’Analyseur de performances (perfmon.exe) pour collecter des informations sur le débit de taux de frame.

- Graphique de RemoteFX - compteurs pour la compression de graphiques de protocole Bureau à distance. Par exemple, si vous souhaitez examiner le nombre de frames en cours de présentation pour le protocole RDP pour la compression, examinez le **entrée Frames/Second** compteur.
- Réseau de RemoteFX - compteurs pour le trafic réseau de protocole Bureau à distance. Par exemple, **temps d’aller-retour (RTT)**.
- Gestion de GPU RemoteFX Root - mesures VRAM disponible et réservée.
- Logiciel de RemoteFX - fournit des compteurs de fréquence de capture, les temps de réponse GPU et d’autres utilisateurs.

Pour plus de bas niveau analyse des performances, en particulier pour la résolution des problèmes, vous pouvez utiliser les compteurs de performances supplémentaires suivants :

- Périphérique de machine virtuelle VSC Synth3D de RemoteFX 
- Canal de Transport de machine virtuelle VSC Synth3D de RemoteFX 
- RemoteFX Synth3D VSP 
- Périphérique de machine virtuelle VSP Synth3D de RemoteFX 
- Canal de machine virtuelle VSP Synth3D de RemoteFX

---
title: Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?
description: Fournit des considérations relatives à la prise en charge tels que les méthodes de démarrage et d’autres différences de fonctionnalités pour vous aider à choisir la génération répond à vos besoins.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: 48319e057da9c815a77349bba34996f89973d85a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850500"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!WARNING]
> Si vous prévoyez de télécharger jamais un Windows virtual machines virtuelles en local vers Microsoft Azure, **génération 1 uniquement des machines virtuelles** qui se trouvent dans le format de fichier de disque dur virtuel et que vous disposez d’un disque de taille fixe sont pris en charge. Pour plus d’informations sur le téléchargement d’un disque dur virtuel Windows ou un VHDX, consultez [préparer un disque dur virtuel Windows à charger sur Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

Votre choix pour créer une génération 1 ou un ordinateur virtuel de génération 2 dépend de système d’exploitation invité que vous souhaitez installer et de la méthode de démarrage que vous souhaitez utiliser pour déployer l’ordinateur virtuel. Nous vous recommandons de créer une machine virtuelle de génération 2 pour tirer parti des fonctionnalités comme le démarrage sécurisé, sauf si une des instructions suivantes est remplie :  

- Le disque dur virtuel que vous voulez démarrer à partir de n’est pas [compatible avec UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- Génération 2 ne prend pas en charge le système d’exploitation que vous souhaitez exécuter sur l’ordinateur virtuel.  
- Génération 2 ne prend pas en charge la méthode de démarrage que vous souhaitez utiliser.  

Pour plus d’informations sur quelles sont les fonctionnalités disponibles avec les ordinateurs virtuels de génération 2, consultez [compatibilité des fonctionnalités par génération et invité Hyper-V](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

Vous ne pouvez pas modifier la génération d’une machine virtuelle une fois que vous l’avez créé. Par conséquent, nous recommandons que vous passez en revue les considérations ici, mais aussi choisissez le système d’exploitation, méthode de démarrage et fonctionnalités que vous souhaitez utiliser avant de choisir une génération.  

## <a name="BKMK_OS"></a>Quels systèmes d’exploitation invités sont pris en charge ?

Ordinateurs virtuels de génération 1 prennent en charge la plupart des systèmes d’exploitation invités. Ordinateurs virtuels de génération 2 prennent en charge plus les versions 64 bits de Windows et des versions plus récentes des systèmes d’exploitation Linux et FreeBSD. Utilisez les sections suivantes pour la génération d’ordinateur virtuel prend en charge le système d’exploitation invité à installer.  

- [Prise en charge du système d’exploitation Windows invité](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Windows)  

- [CentOS et prise en charge du système d’exploitation Red Hat Enterprise Linux invité](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_CentOS)  

- [Prise en charge du système d’exploitation invité Debian](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Debian)  

- [Prise en charge du système d’exploitation FreeBSD invité](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_FreeBSD)  

- [Prise en charge du système d’exploitation invité Linux Oracle](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Oracle)  

- [Prise en charge du système d’exploitation SUSE invité](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_SUSE)  

- [Prise en charge du système d’exploitation Ubuntu invité](Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md#BKMK_Ubuntu)  

### <a name="BKMK_Windows"></a>Prise en charge du système d’exploitation Windows invité

Le tableau suivant présente les versions 64 bits de Windows vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.  

|versions 64 bits de Windows|1e génération|2e génération|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

Le tableau suivant présente les versions 32 bits de Windows vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|versions 32 bits de Windows|1e génération|2e génération|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="BKMK_CentOS"></a>CentOS et prise en charge du système d’exploitation Red Hat Enterprise Linux invité

Le tableau suivant indique les versions de Red Hat Enterprise Linux \(RHEL\) et CentOS vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série de 7.x RHEL/CentOS|&#10004;|&#10004;|  
|Série de RHEL/CentOS 6.x|&#10004;|&#10004;<br />**Remarque :** Prise en charge uniquement sur Windows Server 2016 et versions ultérieures.|  
|Série de 5.x RHEL/CentOS|&#10004;| &#10006;|  

Pour plus d’informations, consultez [CentOS et Red Hat Enterprise Linux ordinateurs virtuels sur Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="BKMK_Debian"></a>Prise en charge du système d’exploitation invité Debian  

Le tableau suivant présente les versions de Debian vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série de Debian 7.x|&#10004;| &#10006;|  
|Série de Debian 8.x|&#10004;|&#10004;|  

Pour plus d’informations, consultez [des machines virtuelles Debian sur Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="BKMK_FreeBSD"></a>Prise en charge du système d’exploitation FreeBSD invité

Le tableau suivant présente les versions de FreeBSD vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.  

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 et 10.1|&#10004;| &#10006;|  
|FreeBSD 9.1 et 9.3|&#10004;| &#10006;|  
|8.4 de FreeBSD|&#10004;| &#10006;|  

Pour plus d’informations, consultez [machines virtuelles de FreeBSD sur Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="BKMK_Oracle"></a>Prise en charge du système d’exploitation invité Linux Oracle  

Le tableau suivant présente les versions de la série de noyau Compatible Red Hat vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.  

|Versions de la série de noyau Compatible Red Hat|1e génération|2e génération|  
|---------------------------------------------|----------------|----------------|  
|Série de Oracle Linux 7.x|&#10004;|&#10004;|
|Série de 6.x Oracle Linux|&#10004;| &#10006;|  

Le tableau suivant présente les versions de Unbreakable Enterprise Kernel vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|Versions Unbreakable Enterprise Kernel (UEK)|1e génération|2e génération|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Pour plus d’informations, consultez [les machines virtuelles Oracle Linux sur Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="BKMK_SUSE"></a>Prise en charge du système d’exploitation SUSE invité

Le tableau suivant présente les versions de SUSE vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série de SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Série de SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Open SUSE 12.3|&#10004;| &#10006;|  

Pour plus d’informations, consultez [des machines virtuelles SUSE sur Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="BKMK_Ubuntu"></a>Prise en charge du système d’exploitation Ubuntu invité

Le tableau suivant présente les versions d’Ubuntu vous pouvez utiliser comme système d’exploitation invité pour la génération 1 et les ordinateurs virtuels de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14.04 et versions ultérieures|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Pour plus d’informations, consultez [de machines virtuelles Ubuntu sur Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="BKMK_Boot"></a>Comment puis-je démarrer la machine virtuelle ?

Le tableau suivant présente le méthodes sont prises en charge par la génération 1 et les ordinateurs virtuels de génération 2 de démarrage.  

|Méthode de démarrage|1e génération|2e génération|  
|---------------|----------------|----------------|  
|démarrage PXE avec une carte réseau standard ;| &#10006;|&#10004;|  
|Démarrage PXE à l’aide d’une carte réseau héritée|&#10004;| &#10006;|  
|Démarrage à partir d’un disque dur virtuel de SCSI (. VHDX) ou de DVD virtuel (. ISO)| &#10006;|&#10004;|  
|Démarrer à partir du disque dur virtuel de contrôleur IDE (. Disque dur virtuel) ou de DVD virtuel (. ISO)|&#10004;| &#10006;|  
|Démarrer à partir du lecteur de disquette (. VFD)|&#10004;| &#10006;|  

## <a name="BKMK_Advantages"></a>Quels sont les avantages de l’utilisation d’ordinateurs virtuels de génération 2 ?

Voici quelques-uns des avantages que vous obtenez lorsque vous utilisez une machine virtuelle de génération 2 :  
- **Démarrage sécurisé** il s’agit d’une fonctionnalité qui vérifie le chargeur de démarrage est signé par une autorité approuvée dans la base de données UEFI pour aider à empêcher l’exécution au moment du démarrage microprogrammes, systèmes d’exploitation ou pilotes UEFI. Le démarrage sécurisé est activé par défaut pour les ordinateurs virtuels de génération 2. Si vous avez besoin exécuter un système d’exploitation invité qui n’est pas pris en charge par le démarrage sécurisé, vous pouvez le désactiver après la création de la machine virtuelle.  Pour plus d'informations, voir [Démarrage sécurisé](https://technet.microsoft.com/library/dn486875.aspx).  

    Pour le démarrage sécurisé 2e-machines virtuelles Linux, vous devez choisir le modèle le démarrage sécurisé UEFI autorité de certification lorsque vous créez la machine virtuelle.  

- **Plus grand volume de démarrage** le volume de démarrage maximale pour les machines virtuelles de génération 2 est de 64 To. Cela correspond à la taille de disque maximal pris en charge par un. VHDX. Pour les machines virtuelles de génération 1, le volume de démarrage maximale est de 2 To pour un. VHDX et 2 040 Go pour un. DISQUE DUR VIRTUEL. Pour plus d’informations, consultez [Hyper-V Virtual Hard Disk Format Overview](https://technet.microsoft.com/library/hh831446.aspx).  

 Vous pouvez également voir une légère amélioration des durées d’installation et de démarrage des machines virtuelles avec des machines virtuelles de génération 2.

## <a name="BKMK_DeviceCompare"></a> Quelle est la différence dans la prise en charge de l’appareil ?

Le tableau suivant compare les appareils disponibles entre la génération 1 et les ordinateurs virtuels de génération 2.  

|Périphérique de génération 1|Remplacement de génération 2|Améliorations de génération 2|  
|-----------------------|----------------------------|-----------------------------|  
|Contrôleur IDE|Contrôleur SCSI virtuel|Démarrage à partir d'un fichier .vhdx (taille maximale de 64 To et fonctionnalité de redimensionnement en ligne)|  
|CD-ROM IDE|CD-ROM SCSI virtuel|Prise en charge de 64 périphériques DVD SCSI par contrôleur SCSI.|  
|BIOS hérité|Microprogramme UEFI|Démarrage sécurisé|  
|Carte réseau héritée|Carte réseau synthétique|Démarrage réseau avec IPv4 et IPv6|  
|Contrôleur de disquettes et contrôleur DMA|Aucune prise en charge des contrôleurs de disquettes|N/A|  
|Émetteur/récepteur asynchrone universel (UART) pour ports COM|UART facultatif pour le débogage|Rapidité et fiabilité accrues|  
|Contrôleur de clavier i8042|Entrée basée sur le logiciel|Utilisation moindre des ressources due à l'absence d'émulation. Réduit également la surface d'attaque sur le système d'exploitation invité.|  
|Clavier PS/2|Clavier logiciel|Utilisation moindre des ressources due à l'absence d'émulation. Réduit également la surface d'attaque sur le système d'exploitation invité.|  
|Souris PS/2|Souris logicielle|Utilisation moindre des ressources due à l'absence d'émulation. Réduit également la surface d'attaque sur le système d'exploitation invité.|  
|Vidéo S3|Vidéo basée sur le logiciel|Utilisation moindre des ressources due à l'absence d'émulation. Réduit également la surface d'attaque sur le système d'exploitation invité.|  
|Bus PCI|Plus nécessaire|N/A|  
|Contrôleur d'interruptions programmable (PIC)|Plus nécessaire|N/A|  
|Minuteur d'intervalle programmable (PIT)|Plus nécessaire|N/A|  
|Super périphérique d'E/S|Plus nécessaire|N/A|  

## <a name="BKMK_More"></a> Plus d’informations sur les ordinateurs virtuels de génération 2

Voici quelques conseils supplémentaires sur l’utilisation d’ordinateurs virtuels de génération 2.

### <a name="attach-or-add-a-dvd-drive"></a>Attacher ou ajouter un lecteur de DVD

- Vous ne pouvez pas attacher un lecteur de CD ou DVD physique à une machine virtuelle de génération 2. Le lecteur de DVD virtuel sur les ordinateurs virtuels de génération 2 ne prend en charge que les fichiers image ISO. Pour créer un fichier image ISO d’un environnement Windows, vous pouvez utiliser l’outil en ligne de commande Oscdimg. Pour plus d’informations, voir [Options de ligne de commande Oscdimg](https://msdn.microsoft.com/library/hh824847.aspx).
- Lorsque vous créez une machine virtuelle avec l’applet de commande New-VM Windows PowerShell, la machine virtuelle de génération 2 n’a pas un lecteur de DVD. Vous pouvez ajouter un lecteur de DVD pendant l’exécution de la machine virtuelle.

### <a name="use-uefi-firmware"></a>Utilisent le microprogramme UEFI

- Démarrage sécurisé ou le microprogramme UEFI n’est pas requis sur l’hôte Hyper-V physique. Hyper-V fournit le microprogramme virtuel aux machines virtuelles qui est indépendant de ce qui est sur l’ordinateur hôte Hyper-V.
- Microprogramme UEFI dans une machine virtuelle de génération 2 ne prend pas en charge le mode d’installation pour le démarrage sécurisé.
- Nous n’en charge l’exécution d’un shell UEFI ou autre application UEFI sur une machine virtuelle de génération 2. L'utilisation d'un shell ou d'une application UEFI non-Microsoft est techniquement possible s'il (ou elle) est compilé(e) directement à partir des sources. Si ces applications ne sont pas correctement signées numériquement, vous devez désactiver le démarrage sécurisé pour la machine virtuelle.

### <a name="work-with-vhdx-files"></a>Travailler avec des fichiers VHDX

- Vous pouvez redimensionner un fichier VHDX qui contient le volume de démarrage pour un ordinateur virtuel de génération 2 pendant l’exécution de la machine virtuelle.
- Nous ne prennent en charge et ne vous recommandons de créer un fichier VHDX est amorçable pour la génération 1 et machines virtuelles de génération 2.  
- La génération de l’ordinateur virtuel est une propriété de l’ordinateur virtuel, et non du disque dur virtuel. Par conséquent, vous ne savez pas si un fichier VHDX a été créé par une génération 1 ou une machine virtuelle de génération 2.  
- Un fichier VHDX est créé avec une génération 2 machines virtuelles peut être attaché au contrôleur IDE ou SCSI d’une machine virtuelle de génération 1. Toutefois, s’il s’agit d’un fichier VHDX de démarrage, la machine virtuelle de génération 1 ne démarre pas.

### <a name="use-ipv6-instead-of-ipv4"></a>Utilise IPv6 plutôt qu’IPv4

Par défaut, les ordinateurs virtuels de génération 2 utilisent IPv4. Pour utiliser IPv6 au lieu de cela, exécutez le [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) applet de commande Windows PowerShell. Par exemple, la commande suivante définit le protocole préféré pour IPv6 pour un ordinateur virtuel nommé TestVM :  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="BKMK_Debug"></a>Ajouter un port COM pour le débogage du noyau

Les ports COM ne sont pas disponibles dans les ordinateurs virtuels de génération 2 jusqu'à ce que vous les ajoutez. Vous pouvez le faire avec Windows PowerShell ou Windows Management Instrumentation (WMI). Ces étapes vous montrent comment le faire avec Windows PowerShell.

Pour ajouter un port COM :  

1. Désactivez le démarrage sécurisé. Débogage du noyau n’est pas compatible avec le démarrage sécurisé. Assurez-vous que l’ordinateur virtuel est dans un état désactivé, puis utiliser le [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) applet de commande. Par exemple, la commande suivante désactive le démarrage sécurisé sur l’ordinateur virtuel TestVM :  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Ajouter un port COM. Utilisez le [Set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) applet de commande pour ce faire. Par exemple, la commande suivante configure le premier port COM sur l’ordinateur virtuel, TestVM, pour se connecter au canal nommé TestPipe sur l’ordinateur local :  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Les ports COM configurés ne sont pas répertoriés dans les paramètres d’un ordinateur virtuel dans le Gestionnaire Hyper-V.

## <a name="see-also"></a>Voir aussi  

- [Linux et les Machines virtuelles de FreeBSD sur Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Utiliser les ressources locales sur l’ordinateur virtuel Hyper-V avec VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Plan pour l’extensibilité d’Hyper-V dans Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)
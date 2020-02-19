---
title: Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?
description: Fournit des considérations telles que les méthodes de démarrage prises en charge et d’autres différences de fonctionnalités pour vous aider à choisir la génération qui répond à vos besoins.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 02e31413-6140-4723-a8d6-46c7f667792d
author: KBDAzure
ms.author: kathydav
ms.date: 12/05/2016
ms.openlocfilehash: fce9b45f538b0d506b621b888d413c99590b1362
ms.sourcegitcommit: 2a15de216edde8b8e240a4aa679dc6d470e4159e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77465553"
---
# <a name="should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v"></a>Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?

>S’applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

> [!NOTE]
> Si vous envisagez de télécharger des machines virtuelles Windows à partir d’un emplacement local vers Microsoft Azure, les machines virtuelles de génération 1 et de génération 2 dans le format de fichier VHD et qui disposent d’un disque de taille fixe sont prises en charge. Consultez [machines virtuelles de génération 2 sur Azure](https://docs.microsoft.com/azure/virtual-machines/windows/generation-2) pour en savoir plus sur les fonctionnalités de génération 2 prises en charge sur Azure. Pour plus d’informations sur le téléchargement d’un disque dur virtuel Windows ou VHDX, consultez [préparer un disque dur virtuel Windows ou un vhdx à charger sur Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

Votre choix de créer une machine virtuelle de génération 1 ou de génération 2 dépend du système d’exploitation invité que vous souhaitez installer et de la méthode de démarrage que vous souhaitez utiliser pour déployer l’ordinateur virtuel. Nous vous recommandons de créer une machine virtuelle de génération 2 pour tirer parti des fonctionnalités telles que le démarrage sécurisé, sauf si l’une des affirmations suivantes est vraie :  

- Le disque dur virtuel à partir duquel vous voulez démarrer n’est pas [compatible avec UEFI](https://technet.microsoft.com/library/hh824898.aspx).  
- La génération 2 ne prend pas en charge le système d’exploitation que vous souhaitez exécuter sur la machine virtuelle.  
- La génération 2 ne prend pas en charge la méthode de démarrage que vous souhaitez utiliser.  

Pour plus d’informations sur les fonctionnalités disponibles avec les ordinateurs virtuels de génération 2, consultez [compatibilité des fonctionnalités Hyper-V par génération et invité](../Hyper-V-feature-compatibility-by-generation-and-guest.md).

Vous ne pouvez pas modifier la génération d’un ordinateur virtuel après l’avoir créé. Nous vous recommandons donc de passer en revue les considérations ici, ainsi que de choisir le système d’exploitation, la méthode de démarrage et les fonctionnalités que vous souhaitez utiliser avant de choisir une génération.  

## <a name="which-guest-operating-systems-are-supported"></a>Quels sont les systèmes d’exploitation invités pris en charge ?

Les ordinateurs virtuels de génération 1 prennent en charge la plupart des systèmes d’exploitation invités. Les ordinateurs virtuels de 2e génération prennent en charge la plupart des versions 64 bits de Windows, ainsi que des versions plus récentes des systèmes d’exploitation Linux et FreeBSD. Utilisez les sections suivantes pour déterminer la génération de la machine virtuelle qui prend en charge le système d’exploitation invité que vous souhaitez installer.  

- [Prise en charge du système d’exploitation invité Windows](#windows-guest-operating-system-support)  

- [Prise en charge des systèmes d’exploitation invités CentOS et Red Hat Enterprise Linux](#centos-and-red-hat-enterprise-linux-guest-operating-system-support)  

- [Prise en charge du système d’exploitation invité Debian](#debian-guest-operating-system-support)  

- [Prise en charge du système d’exploitation invité FreeBSD](#freebsd-guest-operating-system-support)  

- [Oracle Linux la prise en charge du système d’exploitation invité](#oracle-linux-guest-operating-system-support)  

- [Prise en charge du système d’exploitation invité SUSE](#suse-guest-operating-system-support)  

- [Prise en charge du système d’exploitation invité Ubuntu](#ubuntu-guest-operating-system-support)  

### <a name="windows-guest-operating-system-support"></a>Prise en charge du système d’exploitation invité Windows

Le tableau suivant répertorie les versions 64 bits de Windows que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et 2.  

|versions 64 bits de Windows|1e génération|2e génération|  
|-------------------------------|----------------|----------------|  
| Windows Server 2019 |&#10004;|&#10004;|  
| Windows Server 2016 |&#10004;|&#10004;|  
| Windows Server 2012 R2 |&#10004;|&#10004;|  
| Windows Server 2012 |&#10004;|&#10004;|  
|Windows Server 2008 R2|&#10004;| &#10006;|  
|Windows Server 2008|&#10004;| &#10006;|  
|Windows 10|&#10004;|&#10004;|  
|Windows 8.1|&#10004;|&#10004;|  
|Windows 8|&#10004;|&#10004;|  
|Windows 7|&#10004;| &#10006;|

Le tableau suivant répertorie les versions 32 bits de Windows que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et 2.

|versions 32 bits de Windows|1e génération|2e génération|  
|-------------------------------|----------------|----------------|  
|Windows 10|&#10004;| &#10006;|  
|Windows 8.1|&#10004;| &#10006;|  
|Windows 8|&#10004;| &#10006;|  
|Windows 7|&#10004;| &#10006;|  

### <a name="centos-and-red-hat-enterprise-linux-guest-operating-system-support"></a>Prise en charge des systèmes d’exploitation invités CentOS et Red Hat Enterprise Linux

Le tableau suivant indique les versions de Red Hat Enterprise Linux \(RHEL\) et CentOS que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série RHEL/CentOS 7. x|&#10004;|&#10004;|  
|Série RHEL/CentOS 6. x|&#10004;|&#10004;<br />**Remarque :** Pris en charge uniquement sur Windows Server 2016 et versions ultérieures.|  
|Série RHEL/CentOS 5. x|&#10004;| &#10006;|  

Pour plus d’informations, consultez [CentOS et Red Hat Enterprise Linux des machines virtuelles sur Hyper-V](../Supported-CentOS-and-Red-Hat-Enterprise-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="debian-guest-operating-system-support"></a>Prise en charge du système d’exploitation invité Debian  

Le tableau suivant répertorie les versions de Debian que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série Debian 7. x|&#10004;| &#10006;|  
|Série Debian 8. x|&#10004;|&#10004;|  

Pour plus d’informations, consultez [machines virtuelles Debian sur Hyper-V](../Supported-Debian-virtual-machines-on-Hyper-V.md).  

### <a name="freebsd-guest-operating-system-support"></a>Prise en charge du système d’exploitation invité FreeBSD

Le tableau suivant répertorie les versions de FreeBSD que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.  

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|FreeBSD 10 et 10,1|&#10004;| &#10006;|  
|FreeBSD 9,1 et 9,3|&#10004;| &#10006;|  
|FreeBSD 8,4|&#10004;| &#10006;|  

Pour plus d’informations, consultez [machines virtuelles FreeBSD sur Hyper-V](../Supported-FreeBSD-virtual-machines-on-Hyper-V.md).  

### <a name="oracle-linux-guest-operating-system-support"></a>Oracle Linux la prise en charge du système d’exploitation invité  

Le tableau suivant répertorie les versions de la série de noyaux compatibles Red Hat que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.  

|Versions de la série de noyaux compatibles Red Hat|1e génération|2e génération|  
|---------------------------------------------|----------------|----------------|  
|Série Oracle Linux 7. x|&#10004;|&#10004;|
|Série de Oracle Linux 6. x|&#10004;| &#10006;|  

Le tableau suivant indique les versions du noyau d’entreprise qui peuvent être utilisées en tant que système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.

|Versions du noyau d’entreprise (UEK)|1e génération|2e génération|  
|--------------------------------------------------|----------------|----------------|  
|Oracle Linux UEK R3 QU3|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU2|&#10004;| &#10006;|  
|Oracle Linux UEK R3 QU1|&#10004;| &#10006;|  

Pour plus d’informations, consultez [Oracle Linux des machines virtuelles sur Hyper-V](../Supported-Oracle-Linux-virtual-machines-on-Hyper-V.md).  

### <a name="suse-guest-operating-system-support"></a>Prise en charge du système d’exploitation invité SUSE

Le tableau suivant indique les versions de SUSE que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Série SUSE Linux Enterprise Server 12|&#10004;|&#10004;|  
|Série SUSE Linux Enterprise Server 11|&#10004;| &#10006;|  
|Ouvrir SUSE 12,3|&#10004;| &#10006;|  

Pour plus d’informations, consultez [machines virtuelles Suse sur Hyper-V](../Supported-SUSE-virtual-machines-on-Hyper-V.md).  

### <a name="ubuntu-guest-operating-system-support"></a>Prise en charge du système d’exploitation invité Ubuntu

Le tableau suivant indique les versions de Ubuntu que vous pouvez utiliser comme système d’exploitation invité pour les ordinateurs virtuels de génération 1 et de génération 2.

|Versions de système d’exploitation|1e génération|2e génération|  
|-----------------------------|----------------|----------------|  
|Ubuntu 14,04 et versions ultérieures|&#10004;|&#10004;|  
|Ubuntu 12.04|&#10004;| &#10006;|  

Pour plus d’informations, consultez [machines virtuelles Ubuntu sur Hyper-V](../Supported-Ubuntu-virtual-machines-on-Hyper-V.md).  

## <a name="how-can-i-boot-the-virtual-machine"></a>Comment puis-je démarrer l’ordinateur virtuel ?

Le tableau suivant indique les méthodes de démarrage prises en charge par les ordinateurs virtuels de génération 1 et de génération 2.  

|Méthode de démarrage|1e génération|2e génération|  
|---------------|----------------|----------------|  
|démarrage PXE avec une carte réseau standard ;| &#10006;|&#10004;|  
|Démarrage PXE à l’aide d’une carte réseau héritée|&#10004;| &#10006;|  
|Démarrez à partir d’un disque dur virtuel SCSI (. VHDX) ou DVD virtuel (. ISO| &#10006;|&#10004;|  
|Démarrage à partir du disque dur virtuel du contrôleur IDE (. VHD) ou DVD virtuel (. ISO|&#10004;| &#10006;|  
|Démarrage à partir d’une disquette (. VFD|&#10004;| &#10006;|  

## <a name="what-are-the-advantages-of-using-generation-2-virtual-machines"></a>Quels sont les avantages de l’utilisation d’ordinateurs virtuels de génération 2 ?

Voici quelques-uns des avantages que vous pouvez obtenir lorsque vous utilisez un ordinateur virtuel de 2e génération :  
- **Démarrage sécurisé** Il s’agit d’une fonctionnalité qui vérifie que le chargeur de démarrage est signé par une autorité de confiance dans la base de données UEFI afin d’empêcher l’exécution des microprogrammes, systèmes d’exploitation ou pilotes UEFI non autorisés au moment du démarrage. Le démarrage sécurisé est activé par défaut pour les ordinateurs virtuels de génération 2. Si vous devez exécuter un système d’exploitation invité qui n’est pas pris en charge par le démarrage sécurisé, vous pouvez le désactiver après la création de l’ordinateur virtuel.  Pour plus d'informations, voir [Démarrage sécurisé](https://technet.microsoft.com/library/dn486875.aspx).  

    Pour sécuriser les machines virtuelles Linux de génération de démarrage 2, vous devez choisir le modèle de démarrage sécurisé de l’autorité de certification UEFI lors de la création de la machine virtuelle.  

- **Plus grand volume de démarrage** Le volume de démarrage maximal pour les ordinateurs virtuels de génération 2 est de 64 to. Il s’agit de la taille de disque maximale prise en charge par un. VHDX. Pour les ordinateurs virtuels de génération 1, le volume de démarrage maximal est de 2 to pour un. VHDX et 2040GB pour un. Virtuels. Pour plus d’informations, consultez [vue d’ensemble du format de disque dur virtuel Hyper-V](https://technet.microsoft.com/library/hh831446.aspx).  

  Vous pouvez également constater une légère amélioration des durées d’installation et de démarrage des machines virtuelles avec les ordinateurs virtuels de 2e génération.

## <a name="whats-the-difference-in-device-support"></a>Quelle est la différence en matière de prise en charge des appareils ?

Le tableau suivant compare les périphériques disponibles entre les ordinateurs virtuels de génération 1 et de génération 2.  

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

## <a name="more-about-generation-2-virtual-machines"></a>En savoir plus sur les machines virtuelles de génération 2

Voici quelques conseils supplémentaires sur l’utilisation des ordinateurs virtuels de génération 2.

### <a name="attach-or-add-a-dvd-drive"></a>Attacher ou ajouter un lecteur de DVD

- Vous ne pouvez pas attacher un lecteur de CD ou DVD physique à un ordinateur virtuel de 2e génération. Le lecteur de DVD virtuel sur les ordinateurs virtuels de génération 2 ne prend en charge que les fichiers image ISO. Pour créer un fichier image ISO d’un environnement Windows, vous pouvez utiliser l’outil en ligne de commande Oscdimg. Pour plus d’informations, voir [Options de ligne de commande Oscdimg](https://msdn.microsoft.com/library/hh824847.aspx).
- Lorsque vous créez un nouvel ordinateur virtuel avec l’applet de commande Windows PowerShell New-VM, l’ordinateur virtuel de génération 2 ne dispose pas d’un lecteur de DVD. Vous pouvez ajouter un lecteur de DVD pendant que l’ordinateur virtuel est en cours d’exécution.

### <a name="use-uefi-firmware"></a>Utiliser le microprogramme UEFI

- Le démarrage sécurisé ou le microprogramme UEFI n’est pas requis sur l’hôte Hyper-V physique. Hyper-V fournit le microprogramme virtuel aux machines virtuelles qui est indépendant de ce qui se trouve sur l’hôte Hyper-V.
- Le microprogramme UEFI d’un ordinateur virtuel de génération 2 ne prend pas en charge le mode d’installation pour le démarrage sécurisé.
- Nous ne prenons pas en charge l’exécution d’un shell UEFI ou d’autres applications UEFI sur un ordinateur virtuel de génération 2. L'utilisation d'un shell ou d'une application UEFI non-Microsoft est techniquement possible s'il (ou elle) est compilé(e) directement à partir des sources. Si ces applications ne sont pas correctement signées numériquement, vous devez désactiver le démarrage sécurisé pour l’ordinateur virtuel.

### <a name="work-with-vhdx-files"></a>Utiliser des fichiers VHDX

- Vous pouvez redimensionner un fichier VHDX qui contient le volume de démarrage d’un ordinateur virtuel de génération 2 pendant que l’ordinateur virtuel est en cours d’exécution.
- Nous ne prenons pas en charge ou ne recommandons pas la création d’un fichier VHDX démarrable sur les ordinateurs virtuels de génération 1 et de génération 2.  
- La génération de l’ordinateur virtuel est une propriété de l’ordinateur virtuel, et non du disque dur virtuel. Vous ne pouvez donc pas savoir si un fichier VHDX a été créé par un ordinateur virtuel de génération 1 ou de génération 2.  
- Un fichier VHDX créé avec un ordinateur virtuel de génération 2 peut être attaché au contrôleur IDE ou au contrôleur SCSI d’un ordinateur virtuel de génération 1. Toutefois, s’il s’agit d’un fichier VHDX démarrable, l’ordinateur virtuel de génération 1 ne démarre pas.

### <a name="use-ipv6-instead-of-ipv4"></a>Utiliser IPv6 au lieu de IPv4

Par défaut, les ordinateurs virtuels de génération 2 utilisent IPv4. Pour utiliser IPv6 à la place, exécutez l’applet de commande Windows PowerShell [Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) . Par exemple, la commande suivante définit le protocole préféré sur IPv6 pour un ordinateur virtuel nommé TestVM :  

```powershell
Set-VMFirmware -VMName TestVM -IPProtocolPreference IPv6  
```  

## <a name="add-a-com-port-for-kernel-debugging"></a>Ajouter un port COM pour le débogage du noyau

Les ports COM ne sont pas disponibles dans les ordinateurs virtuels de génération 2 tant que vous ne les avez pas ajoutés. Pour ce faire, vous pouvez utiliser Windows PowerShell ou Windows Management Instrumentation (WMI). Ces étapes vous montrent comment effectuer cette opération avec Windows PowerShell.

Pour ajouter un port COM :  

1. Désactivez le démarrage sécurisé. Le débogage du noyau n’est pas compatible avec le démarrage sécurisé. Assurez-vous que l’ordinateur virtuel est à l’état désactivé, puis utilisez l’applet [de commande Set-VMFirmware](https://technet.microsoft.com/library/dn464287.aspx) . Par exemple, la commande suivante désactive le démarrage sécurisé sur la machine virtuelle TestVM :  

    ```powershell  
    Set-VMFirmware -Vmname TestVM -EnableSecureBoot Off  
    ```  

2. Ajoutez un port COM. Pour ce faire, utilisez l’applet de commande [Set-VMComPort](https://technet.microsoft.com/library/hh848616.aspx) . Par exemple, la commande suivante configure le premier port COM sur l’ordinateur virtuel, TestVM, pour se connecter au canal nommé, TestPipe, sur l’ordinateur local :  

    ```powershell
    Set-VMComPort -VMName TestVM 1 \\.\pipe\TestPipe  
    ```  

> [!NOTE]  
> Les ports COM configurés ne sont pas listés dans les paramètres d’un ordinateur virtuel dans le Gestionnaire Hyper-V.

## <a name="see-also"></a>Voir aussi  

- [Machines virtuelles Linux et FreeBSD sur Hyper-V](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md)
- [Utiliser des ressources locales sur un ordinateur virtuel Hyper-V avec VMConnect](../learn-more/Use-local-resources-on-Hyper-V-virtual-machine-with-VMConnect.md)
- [Planifier l’extensibilité d’Hyper-V dans Windows Server 2016](Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md)

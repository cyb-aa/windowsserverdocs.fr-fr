---
title: Créer une machine virtuelle dans Hyper-V
description: Fournit des instructions sur la création d’une machine virtuelle à l’aide du Gestionnaire Hyper-V ou de Windows PowerShell
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 739691650ce3cda8066e9f7ac77626f53f22affa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364248"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Créer une machine virtuelle dans Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Apprenez à créer une machine virtuelle à l’aide du Gestionnaire Hyper-V et de Windows PowerShell, ainsi que des options dont vous disposez lorsque vous créez une machine virtuelle dans le Gestionnaire Hyper-V.  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>Créer une machine virtuelle à l’aide du Gestionnaire Hyper-V  

1.  Ouvrez **le Gestionnaire Hyper-V**.  

2.  Dans le volet **action** , cliquez sur **nouveau**, puis sur **ordinateur virtuel**.  

3.  Dans l' **Assistant nouvel ordinateur virtuel**, cliquez sur **suivant**.  

4.  Effectuez les choix appropriés pour votre machine virtuelle sur chacune des pages. Pour plus d’informations, consultez [nouvelles options et valeurs par défaut des machines virtuelles dans le Gestionnaire Hyper-V](#options-in-hyper-v-manager-new-virtual-machine-wizard) , plus loin dans cette rubrique.  

5.  Après avoir vérifié vos choix dans la page **Résumé** , cliquez sur **Terminer**.  

6.  Dans le Gestionnaire Hyper-V, cliquez avec le bouton droit sur la machine virtuelle, puis sélectionnez **se connecter**.  

7.  Dans la fenêtre connexion à l’ordinateur virtuel, sélectionnez **Action** > **Démarrer**.  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>Créer une machine virtuelle à l’aide de Windows PowerShell  

1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  

2. Cliquez avec le bouton droit sur **Windows PowerShell** , puis sélectionnez **exécuter en tant qu’administrateur**.  

3. Récupérez le nom du commutateur virtuel que vous souhaitez que l’ordinateur virtuel utilise à l’aide de l’option [obtenir-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Par exemple,  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. Utilisez l’applet de commande [New-VM](https://technet.microsoft.com/library/hh848537.aspx) pour créer la machine virtuelle.  Consultez les exemples suivants.  

   > [!NOTE]  
   > Si vous pouvez déplacer cet ordinateur virtuel vers un ordinateur hôte Hyper-V qui exécute Windows Server 2012 R2, utilisez le paramètre-version avec [New-VM](https://technet.microsoft.com/library/hh848537.aspx) pour définir la version de configuration de l’ordinateur virtuel sur 5. La version de configuration de machine virtuelle par défaut pour Windows Server 2016 n’est pas prise en charge par Windows Server 2012 R2 ou les versions antérieures. Vous ne pouvez pas modifier la version de la configuration de la machine virtuelle une fois la machine virtuelle créée. Pour plus d’informations, consultez [versions de configuration des machines virtuelles prises en charge](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions).  

   - **Disque dur virtuel existant** : pour créer une machine virtuelle avec un disque dur virtuel existant, vous pouvez utiliser la commande suivante, où  
     - **-Name** est le nom que vous fournissez pour la machine virtuelle que vous créez.  
     - **-MemoryStartupBytes** est la quantité de mémoire disponible pour la machine virtuelle au démarrage.  
     - **-BootDevice** est l’appareil sur lequel l’ordinateur virtuel démarre lorsqu’il démarre comme la carte réseau (NetworkAdapter) ou le disque dur virtuel (VHD).  
     - **-VHDPath** est le chemin d’accès au disque de la machine virtuelle que vous souhaitez utiliser.  
     - **-Path** est le chemin d’accès pour stocker les fichiers de configuration d’ordinateur virtuel.  
     - **-La génération** est la génération d’ordinateur virtuel. Utilisez la génération 1 pour VHD et la génération 2 pour VHDX. Consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Switch** est le nom du commutateur virtuel que vous souhaitez que l’ordinateur virtuel utilise pour se connecter à d’autres machines virtuelles ou au réseau. Consultez [créer un commutateur virtuel pour les machines virtuelles Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       Exemple :  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       Cela crée un ordinateur virtuel de génération 2 nommé Win10VM avec 4 Go de mémoire. Il démarre à partir du dossier VMs\Win10.vhdx dans le répertoire actif et utilise le commutateur virtuel nommé ExternalSwitch. Les fichiers de configuration d’ordinateur virtuel sont stockés dans le dossier VMData.  

   - **Nouveau disque dur virtuel** : pour créer un ordinateur virtuel avec un nouveau disque dur virtuel, remplacez le paramètre **-VHDPath** de l’exemple ci-dessus par **-NewVHDPath** et ajoutez le paramètre **-NewVHDSizeBytes** . Par exemple,  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **Nouveau disque dur virtuel qui démarre l’image du système d’exploitation** : pour créer un ordinateur virtuel avec un nouveau disque virtuel qui démarre sur une image du système d’exploitation, consultez l’exemple PowerShell dans [créer la procédure pas à pas pour Hyper-V sur Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5. Démarrez la machine virtuelle à l’aide de l’applet de commande [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) . Exécutez l’applet de commande suivante, où nom est le nom de la machine virtuelle que vous avez créée.  

   ```  
   Start-VM -Name <Name>  
   ```  

   Exemple :  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. Connectez-vous à la machine virtuelle à l’aide de la connexion à un ordinateur virtuel (VMConnect).  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Options de l’Assistant nouvel ordinateur virtuel du Gestionnaire Hyper-V  
Le tableau suivant répertorie les options que vous pouvez choisir lorsque vous créez une machine virtuelle dans le Gestionnaire Hyper-V et les valeurs par défaut pour chacune d’elles.  

|Page|Valeur par défaut pour Windows Server 2016 et Windows 10|Autres options|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Spécifier le nom et l’emplacement**|Nom :  Nouvel ordinateur virtuel.<br /><br />Emplacement :  **C:\ProgramData\Microsoft\Windows\Hyper-V @ no__t-1**.|Vous pouvez également entrer votre propre nom et choisir un autre emplacement pour la machine virtuelle.<br /><br />Il s’agit de l’emplacement de stockage des fichiers de configuration de l’ordinateur virtuel.|  
|**Spécifier la génération**|1e génération|Vous pouvez également choisir de créer une machine virtuelle de génération 2. Pour plus d’informations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Affecter la mémoire**|Mémoire de démarrage : 1024 MO<br /><br />Mémoire dynamique : **non sélectionnée**|Vous pouvez définir la mémoire de démarrage de 32 Mo à 5902MB.<br /><br />Vous pouvez également choisir d’utiliser Mémoire dynamique. Pour plus d’informations, consultez [vue d’ensemble d’Hyper-V mémoire dynamique](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurer la mise en réseau**|Non connecté|Vous pouvez sélectionner une connexion réseau à utiliser par la machine virtuelle à partir d’une liste de commutateurs virtuels existants. Consultez [créer un commutateur virtuel pour les machines virtuelles Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Connecter un disque dur virtuel**|Créer un disque dur virtuel<br /><br />Nom : <*vmname*>. vhdx<br /><br />**Emplacement** : **Disques durs C:\users\public\documents\hyper-v\virtual Hard Disks @ no__t-1**<br /><br />**Taille** : 127 GO|Vous pouvez également choisir d’utiliser un disque dur virtuel existant ou d’attendre et d’attacher un disque dur virtuel ultérieurement.|  
|**Options d’installation**|Installer un système d’exploitation ultérieurement|Ces options modifient l’ordre de démarrage de l’ordinateur virtuel afin que vous puissiez l’installer à partir d’un fichier. ISO, d’une disquette de démarrage ou d’un service d’installation réseau, comme les services de déploiement Windows (WDS).|  
|**Résumé**|Affiche les options que vous avez choisies, afin que vous puissiez vérifier qu’elles sont correctes.<br /><br />-Name<br />-Génération<br />-Mémoire<br />-Réseau<br />-Disque dur<br />-Système d’exploitation|**Accélératrice** Vous pouvez copier le résumé de la page et le coller dans un message électronique ou à un autre emplacement pour vous aider à effectuer le suivi de vos machines virtuelles.|  

## <a name="see-also"></a>Voir aussi  

- [Nouvelle machine virtuelle](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versions de configuration d’ordinateur virtuel prises en charge](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  

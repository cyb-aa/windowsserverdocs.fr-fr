---
title: Créer une machine virtuelle dans Hyper-V
description: Fournit des instructions pour la création d’une machine virtuelle à l’aide du Gestionnaire Hyper-V ou Windows PowerShell
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 59297022-a898-456c-b299-d79cd5860238
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 3b850d19663115b72d809af1ae92a444f5491496
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810425"
---
# <a name="create-a-virtual-machine-in-hyper-v"></a>Créer une machine virtuelle dans Hyper-V

>S'applique à : Windows 10, Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019

Découvrez comment créer une machine virtuelle à l’aide du Gestionnaire Hyper-V et Windows PowerShell et les options que vous ont lorsque vous créez une machine virtuelle dans le Gestionnaire Hyper-V.  

## <a name="create-a-virtual-machine-by-using-hyper-v-manager"></a>Créer une machine virtuelle à l’aide du Gestionnaire Hyper-V  

1.  Ouvrez **Gestionnaire Hyper-V**.  

2.  À partir de la **Action** volet, cliquez sur **New**, puis cliquez sur **Machine virtuelle**.  

3.  À partir de la **Assistant Nouvel ordinateur virtuel**, cliquez sur **suivant**.  

4.  Faire les choix appropriés pour votre machine virtuelle sur chacune des pages. Pour plus d’informations, consultez [nouvelles options de machine virtuelle et les valeurs par défaut dans le Gestionnaire Hyper-V](#options-in-hyper-v-manager-new-virtual-machine-wizard) plus loin dans cette rubrique.  

5.  Après avoir vérifié les choix disponibles dans le **Résumé** , cliquez sur **Terminer**.  

6.  Dans le Gestionnaire Hyper-V, cliquez sur la machine virtuelle et sélectionnez **connecter**.  

7.  Dans la fenêtre de connexion de Machine virtuelle, sélectionnez **Action** > **Démarrer**.  

## <a name="create-a-virtual-machine-by-using-windows-powershell"></a>Créer une machine virtuelle à l’aide de Windows PowerShell  

1. Sur le Bureau Windows, cliquez sur le bouton Démarrer et tapez une partie du nom **Windows PowerShell**.  

2. Avec le bouton droit **Windows PowerShell** et sélectionnez **exécuter en tant qu’administrateur**.  

3. Obtenir le nom du commutateur virtuel que vous souhaitez que la machine virtuelle à utiliser à l’aide de [Get-VMSwitch](https://technet.microsoft.com/library/hh848499.aspx).  Par exemple,  

   ```  
   Get-VMSwitch  * | Format-Table Name  
   ```  

4. Utilisez le [New-VM](https://technet.microsoft.com/library/hh848537.aspx) applet de commande pour créer la machine virtuelle.  Consultez les exemples suivants.  

   > [!NOTE]  
   > Si cet ordinateur virtuel peut atteindre un hôte Hyper-V qui exécute Windows Server 2012 R2, utilisez le paramètre - Version avec [New-VM](https://technet.microsoft.com/library/hh848537.aspx) pour définir la version de configuration de machine virtuelle à 5. La version de configuration de machine virtuelle par défaut pour Windows Server 2016 n’est pas pris en charge par Windows Server 2012 R2 ou versions antérieures. Vous ne pouvez pas modifier la version de configuration de machine virtuelle une fois que la machine virtuelle est créée. Pour plus d’informations, consultez [prise en charge des versions de configuration de machine virtuelle](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions).  

   - **Disque dur virtuel existant** - pour créer une machine virtuelle avec un disque dur virtuel existant, vous pouvez utiliser la commande suivante où,  
     - **-Nom** est le nom que vous fournissez pour la machine virtuelle que vous créez.  
     - **-MemoryStartupBytes** est la quantité de mémoire qui est disponible à la machine virtuelle au démarrage.  
     - **-BootDevice** est l’appareil de l’ordinateur virtuel démarre lorsqu’il démarre comme la carte réseau (carte réseau) ou un disque dur virtuel (VHD).  
     - **-VHDPath** est le chemin d’accès sur le disque de machine virtuelle que vous souhaitez utiliser.  
     - **-Chemin d’accès** est le chemin d’accès pour stocker les fichiers de configuration de machine virtuelle.  
     - **-Génération** est la génération de machine virtuelle. Utilisez la génération 1 pour la génération 2 pour VHDX et de disque dur virtuel. Consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  
     - **-Commutateur** est le nom du commutateur virtuel que vous souhaitez que la machine virtuelle à utiliser pour se connecter à d’autres machines virtuelles ou le réseau. Consultez [créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  

       ```  
       New-VM -Name <Name> -MemoryStartupBytes <Memory> -BootDevice <BootDevice> -VHDPath <VHDPath> -Path <Path> -Generation <Generation> -Switch <SwitchName>  
       ```  

       Exemple :  

       ```  
       New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -VHDPath .\VMs\Win10.vhdx -Path .\VMData -Generation 2 -Switch ExternalSwitch  
       ```  

       Cette opération crée une machine virtuelle de génération 2 nommée Win10VM avec 4 Go de mémoire. Il démarre à partir du dossier VMs\Win10.vhdx dans le répertoire actif et utilise le commutateur virtuel nommé ExternalSwitch. Les fichiers de configuration de machine virtuelle sont stockés dans le dossier VMData.  

   - **Nouveau disque dur virtuel** - pour créer une machine virtuelle avec un nouveau disque dur virtuel, remplacez le **- VHDPath** paramètre à partir de l’exemple ci-dessus avec **- NewVHDPath** et ajoutez le **- NewVHDSizeBytes** paramètre. Par exemple,  

     ```  
     New-VM -Name Win10VM -MemoryStartupBytes 4GB -BootDevice VHD -NewVHDPath .\VMs\Win10.vhdx -Path .\VMData -NewVHDSizeBytes 20GB -Generation 2 -Switch ExternalSwitch  
     ```  

   - **Nouveau disque dur virtuel qui démarre sur l’image de système d’exploitation** - pour créer une machine virtuelle avec un nouveau disque virtuel qui démarre à une image de système d’exploitation, consultez l’exemple PowerShell dans [créer une procédure pas à pas de machine virtuelle pour Hyper-V sur Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_create_vm).  

5. Démarrer l’ordinateur virtuel à l’aide de la [Start-VM](https://technet.microsoft.com/library/hh848589.aspx) applet de commande. Exécutez l’applet de commande suivante où nom est le nom de la machine virtuelle que vous avez créé.  

   ```  
   Start-VM -Name <Name>  
   ```  

   Exemple :  

   ```  
   Start-VM -Name Win10VM  
   ```  

6. Se connecter à la machine virtuelle à l’aide de connexion à une Machine virtuelle (VMConnect).  

   ```  
   VMConnect.exe  
   ```  

## <a name="options-in-hyper-v-manager-new-virtual-machine-wizard"></a>Options de l’Assistant Nouvel ordinateur virtuel de Gestionnaire Hyper-V  
Le tableau suivant répertorie les options que vous pouvez choisir lorsque vous créez une machine virtuelle dans le Gestionnaire Hyper-V et les valeurs par défaut pour chacun.  

|Page|Par défaut pour Windows Server 2016 et Windows 10|Autres options|  
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|  
|**Spécifier le nom et l’emplacement**|Nom :  Nouvelle Machine virtuelle.<br /><br />Emplacement :  **C:\ProgramData\Microsoft\Windows\Hyper-V\\** .|Vous pouvez également entrer votre propre nom et choisissez un autre emplacement pour la machine virtuelle.<br /><br />Il s’agit dans lequel les fichiers de configuration d’ordinateur virtuel seront stockés.|  
|**Spécifier la génération**|1e génération|Vous pouvez également choisir de créer une machine virtuelle de génération 2. Pour plus d’informations, consultez [dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?.](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)|  
|**Affecter la mémoire**|Mémoire de démarrage : 1 024 MO<br /><br />Mémoire dynamique : **ne pas sélectionnée**|Vous pouvez définir la mémoire de démarrage à partir de 32 Mo à 5902 Mo.<br /><br />Vous pouvez également choisir d’utiliser la mémoire dynamique. Pour plus d’informations, consultez [Hyper-V Dynamic Memory Overview](https://technet.microsoft.com/library/hh831766.aspx).|  
|**Configurer la mise en réseau**|Non connecté|Vous pouvez sélectionner une connexion de réseau pour la machine virtuelle à utiliser dans une liste de commutateurs virtuels existants. Consultez [créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).|  
|**Connecter un disque dur virtuel**|Créer un disque dur virtuel<br /><br />Nom : <*vmname*> .vhdx<br /><br />**Emplacement** : **Disques durs C:\Users\Public\Documents\Hyper-V\Virtual\\**<br /><br />**Taille** : 127 GO|Vous pouvez également choisir d’utiliser un disque dur virtuel existant ou attendre et attacher un disque dur virtuel ultérieurement.|  
|**Options d’installation**|Installer un système d’exploitation ultérieurement|Ces options Modifier l’ordre de démarrage de l’ordinateur virtuel afin que vous pouvez installer à partir d’un fichier .iso, d’une disquette de démarrage ou d’un service d’installation réseau, tels que les Services de déploiement Windows (WDS).|  
|**Résumé**|Affiche les options que vous avez choisi, afin que vous puissiez vérifier qu’ils sont corrects.<br /><br />-   Name<br />-Génération<br />-Mémoire<br />-Réseau<br />-Disque<br />-Système d’exploitation|**Conseil :** Vous pouvez copier le résumé de la page et le coller dans le courrier électronique ou un autre emplacement pour vous aider à effectuer le suivi de vos machines virtuelles.|  

## <a name="see-also"></a>Voir aussi  

- [New-VM](https://technet.microsoft.com/library/hh848537.aspx)  

- [Versions de configuration de machine virtuelle prises en charge](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md#supported-virtual-machine-configuration-versions)  

-   [Dois-je créer une machine virtuelle de génération 1 ou 2 dans Hyper-V ?](../plan/Should-I-create-a-generation-1-or-2-virtual-machine-in-Hyper-V.md)  

-   [Créer un commutateur virtuel pour les ordinateurs virtuels Hyper-V](Create-a-virtual-switch-for-Hyper-V-virtual-machines.md)  

---
title: Déployer des appareils graphiques à l’aide de l’affectation discrète des appareils
description: Découvrez comment utiliser DDA pour déployer des appareils graphiques dans Windows Server
ms.prod: windows-server
ms.technology: hyper-v
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.assetid: 67a01889-fa36-4bc6-841d-363d76df6a66
ms.date: 08/21/2019
ms.openlocfilehash: 07f0ba19aaf998bb7b2fe8cf4ef1ba6cf8cae322
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860912"
---
# <a name="deploy-graphics-devices-using-discrete-device-assignment"></a>Déployer des appareils graphiques à l’aide de l’affectation discrète des appareils

> S’applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Windows Server 2019, Microsoft Hyper-V Server 2019  

À compter de Windows Server 2016, vous pouvez utiliser l’affectation discrète des appareils, ou DDA, pour transmettre un appareil PCIe entier à une machine virtuelle.  Cela permet un accès très performant aux appareils tels que le [stockage NVMe](./Deploying-storage-devices-using-dda.md) ou les cartes graphiques à partir d’une machine virtuelle tout en étant en mesure de tirer parti des pilotes natifs des appareils.  Pour plus d’informations sur les appareils qui fonctionnent, sur les implications sur la sécurité, consultez le [plan de déploiement d’appareils à l’aide de l’affectation discrète](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) d’appareils, etc.

Il existe trois étapes pour utiliser un appareil avec l’affectation discrète des appareils :
-   Configurer la machine virtuelle pour l’affectation discrète des appareils
-   Démonter l’appareil de la partition hôte
-   Attribution de l’appareil à la machine virtuelle invitée

Toutes les commandes peuvent être exécutées sur l’hôte d’une console Windows PowerShell en tant qu’administrateur.

## <a name="configure-the-vm-for-dda"></a>Configurer la machine virtuelle pour DDA
L’affectation discrète des appareils impose des restrictions aux machines virtuelles et l’étape suivante doit être effectuée.

1.  Configurez l’action d’arrêt automatique d’une machine virtuelle sur TurnOff en exécutant

```
Set-VM -Name VMName -AutomaticStopAction TurnOff
```

### <a name="some-additional-vm-preparation-is-required-for-graphics-devices"></a>Une préparation supplémentaire des machines virtuelles est nécessaire pour les périphériques graphiques

Le matériel fonctionne mieux si la machine virtuelle est configurée d’une certaine manière.  Pour savoir si vous avez besoin des configurations suivantes pour votre matériel, contactez le fournisseur du matériel. Pour plus d’informations, consultez [planifier le déploiement des appareils à l’aide de l’attribution discrète des appareils](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) et sur ce billet de [blog.](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266)

1. Activer la combinaison d’écriture sur le processeur
   ```
   Set-VM -GuestControlledCacheTypes $true -VMName VMName
   ```
2. Configurer l’espace MMIO 32 bits
   ```
   Set-VM -LowMemoryMappedIoSpace 3Gb -VMName VMName
   ```
3. Configurer un espace de plus de 32 bits MMIO
   ```
   Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName VMName
   ```
   > [!TIP] 
   > Les valeurs d’espace MMIO ci-dessus sont des valeurs raisonnables à définir pour l’expérimentation d’une seule unité GPU.  Si, après le démarrage de la machine virtuelle, l’appareil signale une erreur liée à des ressources insuffisantes, vous devrez probablement modifier ces valeurs. Consultez [planifier le déploiement d’appareils à l’aide de l’attribution discrète des appareils](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md) pour savoir comment calculer précisément les exigences de la spécification MMIO.

## <a name="dismount-the-device-from-the-host-partition"></a>Démonter l’appareil de la partition hôte
### <a name="optional---install-the-partitioning-driver"></a>Facultatif-installer le pilote de partitionnement
L’affectation discrète des appareils offre aux fournisseurs de matériel la possibilité de fournir un pilote d’atténuation de la sécurité avec leurs appareils.  Notez que ce pilote n’est pas le même que le pilote de périphérique qui sera installé dans la machine virtuelle invitée.  C’est à la discrétion du fournisseur de matériel de fournir ce pilote. Toutefois, s’il le fournit, installez-le avant de démonter l’appareil de la partition hôte.  Contactez le fournisseur du matériel pour plus d’informations sur s’il dispose d’un pilote d’atténuation
> Si aucun pilote de partitionnement n’est fourni, pendant le démontage, vous devez utiliser l’option `-force` pour ignorer l’avertissement de sécurité. Pour plus d’informations sur les implications en matière de sécurité, consultez [planifier le déploiement d’appareils à l’aide de l’attribution discrète des](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md)appareils.

### <a name="locating-the-devices-location-path"></a>Recherche du chemin d’accès à l’emplacement de l’appareil
Le chemin d’accès à l’emplacement PCI est requis pour démonter et monter l’appareil à partir de l’ordinateur hôte.  Un exemple de chemin d’accès à l’emplacement ressemble à ce qui suit : `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.  Pour plus d’informations sur le chemin d’accès de l’emplacement, cliquez ici : [planifier le déploiement des appareils à l’aide de l’attribution discrète des appareils](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md).

### <a name="disable-the-device"></a>Désactiver l’appareil
À l’aide de Device Manager ou de PowerShell, assurez-vous que l’appareil est « désactivé ».  

### <a name="dismount-the-device"></a>Démonter l’appareil
Selon que le fournisseur a fourni un pilote d’atténuation, vous devez soit utiliser l’option « -force », soit non.
- Si un pilote d’atténuation a été installé
  ```
  Dismount-VMHostAssignableDevice -LocationPath $locationPath
  ```
- Si aucun pilote d’atténuation n’a été installé
  ```
  Dismount-VMHostAssignableDevice -force -LocationPath $locationPath
  ```

## <a name="assigning-the-device-to-the-guest-vm"></a>Attribution de l’appareil à la machine virtuelle invitée
La dernière étape consiste à dire à Hyper-V qu’une machine virtuelle doit avoir accès à l’appareil.  En plus du chemin d’accès d’emplacement indiqué ci-dessus, vous devez connaître le nom de la machine virtuelle.

```
Add-VMAssignableDevice -LocationPath $locationPath -VMName VMName
```

## <a name="whats-next"></a>Étape suivante
Une fois qu’un appareil est correctement monté sur une machine virtuelle, vous pouvez maintenant démarrer cette machine virtuelle et interagir avec l’appareil comme vous le feriez normalement si vous étiez en train d’exécuter sur un système nu.  Cela signifie que vous êtes maintenant en mesure d’installer les pilotes du fournisseur de matériel dans la machine virtuelle et que les applications peuvent voir ce matériel présent.  Vous pouvez le vérifier en ouvrant le gestionnaire de périphériques sur la machine virtuelle invitée et en vérifiant que le matériel s’affiche à présent.

## <a name="removing-a-device-and-returning-it-to-the-host"></a>Suppression d’un appareil et retour à l’ordinateur hôte
Si vous souhaitez rétablir l’appareil à son état d’origine, vous devez arrêter la machine virtuelle et émettre les informations suivantes :
```
#Remove the device from the VM
Remove-VMAssignableDevice -LocationPath $locationPath -VMName VMName
#Mount the device back in the host
Mount-VMHostAssignableDevice -LocationPath $locationPath
```
Vous pouvez ensuite réactiver l’appareil dans le gestionnaire de périphériques et le système d’exploitation hôte pourra à nouveau interagir avec l’appareil.

## <a name="example"></a>Exemple

### <a name="mounting-a-gpu-to-a-vm"></a>Montage d’un GPU sur une machine virtuelle
Dans cet exemple, nous utilisons PowerShell pour configurer une machine virtuelle nommée « ddatest1 » afin de prendre le premier GPU disponible par le fabricant NVIDIA et de l’affecter à la machine virtuelle.  
```
#Configure the VM for a Discrete Device Assignment
$vm =   "ddatest1"
#Set automatic stop action to TurnOff
Set-VM -Name $vm -AutomaticStopAction TurnOff
#Enable Write-Combining on the CPU
Set-VM -GuestControlledCacheTypes $true -VMName $vm
#Configure 32 bit MMIO space
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
#Configure Greater than 32 bit MMIO space
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm

#Find the Location Path and disable the Device
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
#Disable the PNP Device
Disable-PnpDevice  -InstanceId $gpudevs[0].InstanceId

#Dismount the Device from the Host
Dismount-VMHostAssignableDevice -force -LocationPath $locationPath

#Assign the device to the guest VM.
Add-VMAssignableDevice -LocationPath $locationPath -VMName $vm
```

## <a name="troubleshooting"></a>Résolution des problèmes

Si vous avez passé un GPU à une machine virtuelle mais que Bureau à distance ou une application ne reconnaît pas le GPU, vérifiez les problèmes courants suivants :

- Vérifiez que vous avez installé la version la plus récente du pilote pris en charge par le fournisseur de GPU et que le pilote ne signale pas d’erreurs en vérifiant l’état de l’appareil dans Device Manager.
- Assurez-vous que votre appareil a suffisamment d’espace MMIO alloué au sein de la machine virtuelle. Pour plus d’informations, consultez la section [MMIO Space](../plan/Plan-for-Deploying-Devices-using-Discrete-Device-Assignment.md#mmio-space).
- Assurez-vous que vous utilisez un GPU que le fournisseur prend en charge dans cette configuration. Par exemple, certains fournisseurs empêchent les cartes de leurs consommateurs de fonctionner lorsqu’ils sont passés à une machine virtuelle.
- Assurez-vous que l’application en cours d’exécution prend en charge l’exécution à l’intérieur d’une machine virtuelle, et que le GPU et ses pilotes associés sont pris en charge par l’application. Certaines applications ont des listes verte d’unités GPU et d’environnements.
- Si vous utilisez le rôle d’hôte de session Bureau à distance ou Windows multipoint services sur l’invité, vous devez vous assurer qu’une entrée de stratégie de groupe spécifique est définie pour autoriser l’utilisation du GPU par défaut. À l’aide d’un objet stratégie de groupe appliqué à l’invité (ou à l’éditeur de stratégie de groupe local sur l’invité), accédez à l’élément de stratégie de groupe suivant : configuration de l' **ordinateur** > les **modèles d’administrateur** > les **composants Windows** > services Bureau à distance > Bureau à distance > hôte de **session** > l’environnement de session **à distance** services Bureau à distance **utiliser la carte graphique par défaut du matériel pour toutes les sessions**. **Remote Desktop Services** Définissez cette valeur sur activé, puis redémarrez la machine virtuelle une fois que la stratégie a été appliquée.

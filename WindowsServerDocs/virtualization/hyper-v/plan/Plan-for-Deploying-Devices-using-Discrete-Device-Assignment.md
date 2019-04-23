---
title: Planifier le déploiement des appareils à l’aide d’attribution discrète d’appareil
description: En savoir plus sur le fonctionnement de DDA dans Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: c64c2b75c00f97622278c3e590db46995e108218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840200"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Plan pour les appareils de déploiement à l’aide d’attribution discrète d’appareil
>S'applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

Attribution discrète d’appareil permet de matériel physique de PCIe soient directement accessibles à partir d’un ordinateur virtuel.  Ce guide traite le type de périphériques que pouvez utiliser l’affectation d’appareils discrètes, configuration du système hôte, les limitations imposées sur les machines virtuelles, ainsi que les implications en matière de sécurité d’attribution discrète d’appareil.

Pour la version initiale de l’affectation d’appareil discrète, nous nous sommes concentrés sur les deux classes de périphériques pour être officiellement pris en charge par Microsoft : Cartes graphiques et stockage de NVMe appareils.  Autres périphériques sont sans doute et fabricants de matériel sont en mesure d’offrir des instructions de prise en charge pour ces appareils.  Pour ces autres périphériques, veuillez contacter ces fournisseurs de matériel pour la prise en charge.

Si vous êtes prêt à essayer d’attribution discrète d’appareil, vous pouvez atteindre sur [déploiement Graphics appareils utilisation discrète appareil de l’attribution](../deploy/Deploying-graphics-devices-using-dda.md) ou [déploiement de périphériques de stockage à l’aide d’affectation d’appareils discrètes](../deploy/Deploying-storage-devices-using-dda.md) Pour commencer !

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Les Machines virtuelles prises en charge et les systèmes d’exploitation invités
Affectation discrète d’appareils est prise en charge pour la génération 1 ou 2 machines virtuelles.  En outre, les invités pris en charge incluent Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 avec [Ko 3133690](https://support.microsoft.com/kb/3133690) appliqué et diverses distributions de la [système d’exploitation Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Configuration système
Outre le [System Configuration requise pour Windows Server](../../../get-started/System-Requirements--and-Installation.md) et le [configuration système requise pour Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), attribution discrète d’appareil nécessite le matériel de classe serveur qui est capable d’octroyer le le contrôle de système d’exploitation sur la configuration de la structure de PCIe (PCI Express contrôle natif). En outre, le complexe PCIe racine doit prendre en charge « Access Control Services » (ACS), ce qui permet d’Hyper-V forcer tout le trafic de PCIe via le MMU d’e/s.

Généralement, ces fonctionnalités ne sont pas exposées directement dans le BIOS du serveur et sont souvent masquées derrière les autres paramètres.  Par exemple, les mêmes fonctionnalités sont requises pour la prise en charge SR-IOV et dans le BIOS vous devrez peut-être définir « Activer SR-IOV ».  Veuillez contacter votre fournisseur si vous ne parvenez pas à identifier le paramètre correct dans le BIOS.

Pour garantir le matériel de matériel est capable d’attribution discrète d’appareil, nos ingénieurs ont établi un [Script de profil d’ordinateur](#machine-profile-script) que vous pouvez exécuter sur un ordinateur hôte Hyper-V est activé pour tester si votre serveur est correctement le programme d’installation et ce que les appareils sont capables d’attribution discrète d’appareil.

## <a name="device-requirements"></a>Exigences de l’appareil
Pas tous les périphériques PCIe peuvent être utilisé avec affectation discrète d’appareils.  Par exemple, les appareils plus anciens qui tirent parti de hérité (INTx) les interruptions PCI ne sont pas pris en charge. De Jake Oshin [billets de blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) accédez à plus de détails, toutefois, pour le consommateur, en cours d’exécution le [Script de profil d’ordinateur](#machine-profile-script) affiche les appareils qui sont susceptibles d’être utilisés pour l’attribution de périphérique discrètes.

Les fabricants de périphériques peuvent contacter leur représentant Microsoft pour plus d’informations.

## <a name="device-driver"></a>Pilote de périphérique
Comme de l’attribution discrète d’appareil transmet l’ensemble de l’appareil PCIe dans la machine virtuelle invitée, un pilote de l’hôte n’est pas doit être installé avant de l’appareil qui est monté au sein de la machine virtuelle.  La seule exigence sur l’ordinateur hôte est que l’appareil [chemin de localisation PCIe](#pcie-location-path) peut être déterminé.  Pilote de l’appareil peut éventuellement être installé si cela permet d’identifier l’appareil.  Par exemple, un GPU sans son pilote de périphérique installé sur l’hôte peut apparaître comme un périphérique de rendu base Microsoft.  Si le pilote de périphérique est installé, son fabricant et modèle probablement s’afficheront.

Une fois l’appareil est monté dans l’invité, pilote de périphérique du fabricant peut désormais être installé comme normal à l’intérieur de la machine virtuelle invitée.  

## <a name="virtual-machine-limitations"></a>Limitations de la Machine virtuelle
En raison de la nature comment attribution discrète d’appareil est implémenté, certaines fonctionnalités d’une machine virtuelle sont limitées, même si un appareil est attaché.  Les fonctionnalités suivantes ne sont pas disponibles :
- Sauvegarde/restauration de machine virtuelle
- Migration dynamique d’une machine virtuelle
- L’utilisation de la mémoire dynamique
- Ajout de la machine virtuelle à un cluster à haute disponibilité (HA)

## <a name="security"></a>Sécurité
Attribution discrète d’appareil transmet l’ensemble de l’appareil à la machine virtuelle.  Cela signifie que toutes les fonctionnalités de ces périphériques sont accessibles à partir du système d’exploitation invité. Certaines fonctionnalités, telles que la mise à jour du microprogramme, peuvent nuire à la stabilité du système. Par conséquent, les avertissements de nombreuses sont présentés à l’administrateur lors du démontage de l’appareil à partir de l’hôte. Nous recommandons vivement qu’attribution discrète d’appareil est utilisée uniquement où les locataires des machines virtuelles sont approuvées.  

Si l’administrateur souhaite utiliser un appareil avec un client non approuvé, nous avons fourni des fabricants d’appareils avec la possibilité de créer un pilote de périphérique atténuation qui peut être installé sur l’ordinateur hôte.  Contactez le fabricant du périphérique pour plus d’informations sur si elles fournissent un pilote d’atténuation de périphérique.

Si vous souhaitez ignorer les contrôles de sécurité pour un périphérique qui n’a pas d’un pilote d’atténuation de périphérique, vous devrez passer la `-Force` paramètre à la `Dismount-VMHostAssignableDevice` applet de commande.  Comprendre qu’en procédant ainsi, vous avez modifié le profil de sécurité de ce système et cela est uniquement recommandé lors du prototypage ou approuvé environnements.

## <a name="pcie-location-path"></a>Chemin de localisation PCIe
Le chemin d’accès de l’emplacement de PCIe est requis pour démonter et monter l’appareil à partir de l’hôte.  Un chemin de localisation exemple ressemble à ceci : `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`.   Le [Script de profil d’ordinateur](#machine-profile-script) retournera également le chemin d’accès de l’emplacement de l’appareil PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtenir le chemin d’accès de l’emplacement à l’aide du Gestionnaire de périphériques
![Gestionnaire de périphériques](../deploy/media/dda-devicemanager.png)
- Ouvrez le Gestionnaire de périphériques et recherchez le périphérique.  
- Cliquez avec le bouton droit sur l’appareil et sélectionnez « Propriétés ».
- Accédez à l’onglet Détails et sélectionnez « Chemins de localisation » dans la liste déroulante propriété.  
- Cliquez avec le bouton droit sur l’entrée qui commence par « PCIROOT » et sélectionnez « Copier ».  Vous avez maintenant le chemin d’accès de l’emplacement de cet appareil.

## <a name="mmio-space"></a>Espace MMIO
Certains appareils, en particulier les GPU, nécessitent un espace MMIO supplémentaire soit allouée à la machine virtuelle pour la mémoire de cet appareil pour être accessible. Par défaut, chaque machine virtuelle commence avec un faible espace MMIO 128 Mo et 512 Mo d’espace MMIO haute lui soit alloué. Toutefois, un appareil peut nécessiter davantage d’espace MMIO ou plusieurs appareils peuvent être acheminés à travers telles que les exigences combinées dépassent ces valeurs.  Modification de MMIO espace est très simple et peut être effectuée dans PowerShell à l’aide des commandes suivantes :

```
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```
Le moyen le plus simple pour déterminer la quantité d’espace à allouer MMIO consiste à utiliser le [Script de profil d’ordinateur](#machine-profile-script).  Vous pouvez également calculer à l’aide du Gestionnaire de périphériques. Consultez le blog TechNet valider [discrètes affectation d’appareils - GPU](https://blogs.technet.microsoft.com/virtualization/2015/11/23/discrete-device-assignment-gpus/) pour plus d’informations.

## <a name="machine-profile-script"></a>Script de profil d’ordinateur
Afin de simplifier l’identification si le serveur est configuré correctement et quels appareils sont disponibles pour être acheminé à travers l’utilisation de l’attribution de périphérique discrètes, un de nos ingénieurs rassemblé le script PowerShell suivant : [SurveyDDA.ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Avant d’utiliser le script, vérifiez que vous avez le rôle Hyper-V installé et que vous exécutez le script à partir d’une fenêtre de commande PowerShell qui a des privilèges d’administrateur.

Si le système est correctement configuré pour prendre en charge d’attribution discrète d’appareil, l’outil affiche un message d’erreur quant à ce qui est incorrect. Si l’outil détecte le système configuré correctement, il énumère tous les périphériques qu’il peut trouver sur le PCIe Bus.

Pour chaque appareil qu’il trouve, l’outil affichera qu’il s’agisse peuvent être utilisés avec affectation discrète d’appareils. Si un appareil est identifié comme étant compatible avec affectation discrète d’appareils, le script fournit une raison.  Lorsqu’un appareil est correctement identifié comme étant compatible, chemin d’accès de l’appareil s’affiche.  En outre, si cet appareil nécessite [espace MMIO](#mmio-space), il s’affichera également.

![SurveyDDA.ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Forum Aux Questions

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>Comment la technologie du Bureau à distance RemoteFX vGPU concerne-t-il attribution discrète d’appareil ?
Ils sont complètement distincts des technologies. RemoteFX vGPU n’a pas besoin être installé pour l’attribution discrète appareil fonctionne. En outre, aucun des rôles supplémentaires ne sont requis à installer. RemoteFX vGPU nécessite le rôle RDVH doit être installé afin que le pilote de vGPU RemoteFX soit présent dans la machine virtuelle. Pour affectation discrète d’appareils, dans la mesure où vous allez installer les pilotes du fabricant de matériel à la machine virtuelle, aucun des rôles supplémentaires ne doivent être présents.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>J’ai passé un GPU dans une machine virtuelle, mais le Bureau à distance ou une application n’est pas reconnaître le GPU
Il existe plusieurs raisons à que cela peut se produire, mais plusieurs problèmes courants sont répertoriés ci-dessous.
- Vérifiez que les pilotes du fournisseur GPU plus récente sont installée et ne signalement pas d’une erreur en vérifiant l’état de périphérique dans le Gestionnaire de périphériques.
- Assurez-vous que l’appareil dispose de suffisamment [espace MMIO](#mmio-space) lui est alloué au sein de la machine virtuelle.
- Vérifiez que vous utilisez un GPU prenant en charge le fournisseur utilisé dans cette configuration. Par exemple, certains fournisseurs empêchent leurs cartes consommateur travail lorsque passé à une machine virtuelle.
- Vérifiez que l’application en cours d’exécution prend en charge en cours d’exécution à l’intérieur d’une machine virtuelle, et que le GPU et ses pilotes associés sont pris en charge par l’application. Certaines applications possèdent des listes vertes de GPU et d’environnements.
- Si vous utilisez le rôle de l’hôte de Session Bureau à distance ou Windows Multipoint Services sur l’invité, vous devez vous assurer qu’une entrée spécifique de la stratégie de groupe est définie pour autoriser l’utilisation de la valeur par défaut GPU. À l’aide d’un objet de stratégie de groupe appliquée à l’invité (ou l’éditeur de stratégie de groupe locale sur l’invité), accédez à l’élément de stratégie de groupe suivant :
   - Configuration ordinateur
   - Modèles d’administrateur
   - Composants Windows
   - Services Bureau à distance
   - Hôte de session Bureau à distance
   - Environnement de Session à distance
   - Utiliser l’adaptateur de graphiques de matériel par défaut pour toutes les sessions de Services Bureau à distance

    Définissez cette valeur sur activé, puis redémarrez la machine virtuelle une fois que la stratégie a été appliquée.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>Affectation discrète d’appareils bénéficient de codec de AVC444 du Bureau à distance ?
Oui, consultez ce billet de blog pour plus d’informations : [Améliorations de AVC/H.264 protocole RDP (Desktop) 10 à distance dans Windows 10 et Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>Puis-je utiliser PowerShell pour obtenir le chemin d’accès de l’emplacement ?
Oui, il existe différentes manières de procéder. Voici un exemple :
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>Affectation d’appareils discrètes utilisable pour passer d’un périphérique USB dans une machine virtuelle ?
Bien que pas officiellement pris en charge, nos clients ont utilisé une attribution appareil discrètes pour ce faire, en passant l’ensemble du contrôleur USB3 dans une machine virtuelle.  Comme le contrôleur entier est passé dans, chaque périphérique USB connecté à ce contrôleur seront également accessible dans la machine virtuelle.  Notez que seuls certains contrôleurs USB3 peuvent fonctionner et USB2 contrôleurs ne peuvent pas être utilisés avec affectation discrète d’appareils.

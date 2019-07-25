---
title: Planifier le déploiement d’appareils à l’aide de l’affectation discrète des appareils
description: En savoir plus sur le fonctionnement de DDA dans Windows Server
ms.prod: windows-server-threshold
ms.service: na
ms.technology: hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: chrishuybregts
ms.author: chrihu
ms.date: 02/06/2018
ms.openlocfilehash: 7df7dbd1e7252f5bab451ed9272f9cbede63d223
ms.sourcegitcommit: 216d97ad843d59f12bf0b563b4192b75f66c7742
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68476494"
---
# <a name="plan-for-deploying-devices-using-discrete-device-assignment"></a>Planifier le déploiement d’appareils à l’aide de l’affectation discrète des appareils
>S'applique à : Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019

L’affectation discrète des appareils permet d’accéder directement au matériel PCIe physique à partir d’un ordinateur virtuel.  Ce guide aborde le type d’appareils qui peuvent utiliser l’affectation discrète des appareils, la configuration requise pour l’ordinateur hôte, les limitations imposées aux machines virtuelles et les implications en termes de sécurité de l’affectation discrète des appareils.

Pour la version initiale de l’attribution d’appareils discret, nous nous sommes concentrés sur deux classes d’appareils qui doivent être formellement prises en charge par Microsoft: Les adaptateurs graphiques et les périphériques de stockage NVMe.  D’autres périphériques sont susceptibles de fonctionner et les fournisseurs de matériel peuvent offrir des déclarations de prise en charge pour ces appareils.  Pour ces autres appareils, contactez les fournisseurs de matériel pour obtenir de l’aide.

Si vous êtes prêt à essayer l’attribution discrète des appareils, vous pouvez passer au [déploiement de périphériques graphiques à l’aide d’une attribution d’appareil discrète](../deploy/Deploying-graphics-devices-using-dda.md) ou du déploiement de périphériques de [stockage à l’aide de l’attribution d’appareil discrète](../deploy/Deploying-storage-devices-using-dda.md) pour commencer.

## <a name="supported-virtual-machines-and-guest-operating-systems"></a>Machines virtuelles et systèmes d’exploitation invités pris en charge
L’affectation discrète d’appareils est prise en charge pour les machines virtuelles de génération 1 ou 2.  En outre, les invités pris en charge incluent Windows 10, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 avec [KB 3133690](https://support.microsoft.com/kb/3133690) appliqué et diverses distributions du [système d’exploitation Linux.](../supported-linux-and-freebsd-virtual-machines-for-hyper-v-on-windows.md)

## <a name="system-requirements"></a>Configuration système requise
En plus de la [Configuration système requise pour Windows Server](../../../get-started/System-Requirements--and-Installation.md) et de la [Configuration système requise pour Hyper-V](../System-requirements-for-Hyper-V-on-Windows.md), l’affectation discrète des appareils nécessite un matériel de classe serveur capable d’accorder le contrôle du système d’exploitation à la configuration du PCIe infrastructure (contrôle PCI Express natif). En outre, la racine PCIe complexe doit prendre en charge «services de Access Control» (ACS), qui permet à Hyper-V de forcer tout le trafic PCIe via la MMU d’e/s.

Ces fonctionnalités ne sont généralement pas exposées directement dans le BIOS du serveur et sont souvent masquées derrière d’autres paramètres.  Par exemple, les mêmes fonctionnalités sont requises pour la prise en charge de SR-IOV et dans le BIOS, vous devrez peut-être définir «Enable SR-IOV».  Veuillez contacter votre fournisseur de système si vous ne parvenez pas à identifier le paramètre correct dans votre BIOS.

Pour s’assurer que le matériel est capable d’attribuer des appareils discrets, nos ingénieurs ont rassemblé un [script de profil d’ordinateur](#machine-profile-script) que vous pouvez exécuter sur un hôte Hyper-V pour tester si votre serveur est correctement configuré et quels appareils sont capables de Attribution d’appareil discrète.

## <a name="device-requirements"></a>Configuration requise pour l’appareil
Tous les périphériques PCIe ne peuvent pas être utilisés avec l’affectation discrète des appareils.  Par exemple, les appareils plus anciens qui tirent parti des interruptions PCI héritées (INTx) ne sont pas pris en charge. Les billets de [blog](https://blogs.technet.microsoft.com/virtualization/2015/11/20/discrete-device-assignment-machines-and-devices/) de julien Oshin sont plus détaillés. Toutefois, pour le consommateur, l’exécution du [script de profil d’ordinateur](#machine-profile-script) affiche les appareils qui peuvent être utilisés pour l’affectation discrète des appareils.

Les fabricants de périphériques peuvent contacter leur représentant Microsoft pour plus d’informations.

## <a name="device-driver"></a>Pilote de périphérique
Comme l’attribution d’appareils discrets transmet l’intégralité du périphérique PCIe à la machine virtuelle invitée, il n’est pas nécessaire d’installer un pilote hôte avant le montage de l’appareil sur la machine virtuelle.  La seule exigence sur l’hôte est que le [chemin d’accès à l’emplacement PCIe](#pcie-location-path) de l’appareil peut être déterminé.  Le pilote de l’appareil peut éventuellement être installé si cela permet d’identifier l’appareil.  Par exemple, un GPU sans son pilote de périphérique installé sur l’ordinateur hôte peut apparaître comme un périphérique de rendu de base Microsoft.  Si le pilote de périphérique est installé, son fabricant et son modèle seront probablement affichés.

Une fois que l’appareil est monté à l’intérieur de l’invité, le pilote de périphérique du fabricant peut désormais être installé comme normal à l’intérieur de la machine virtuelle invitée.  

## <a name="virtual-machine-limitations"></a>Limitations des machines virtuelles
En raison de la façon dont l’attribution d’appareils discrètes est implémentée, certaines fonctionnalités d’une machine virtuelle sont restreintes lorsqu’un appareil est connecté.  Les fonctionnalités suivantes ne sont pas disponibles:
- Enregistrer/Restaurer des machines virtuelles
- Migration dynamique d’une machine virtuelle
- Utilisation de la mémoire dynamique
- Ajout de la machine virtuelle à un cluster à haute disponibilité

## <a name="security"></a>Sécurité
L’attribution d’appareil discrète transmet l’intégralité de l’appareil à la machine virtuelle.  Cela signifie que toutes les fonctionnalités de cet appareil sont accessibles depuis le système d’exploitation invité. Certaines fonctionnalités, comme la mise à jour du microprogramme, peuvent avoir un impact négatif sur la stabilité du système. Par conséquent, de nombreux avertissements sont présentés à l’administrateur lors du démontage de l’appareil à partir de l’ordinateur hôte. Nous vous recommandons vivement d’utiliser l’affectation discrète des appareils uniquement lorsque les locataires des machines virtuelles sont approuvées.  

Si l’administrateur souhaite utiliser un appareil avec un locataire non approuvé, nous fournissons des fabriques de périphériques avec la possibilité de créer un pilote d’atténuation des appareils qui peut être installé sur l’ordinateur hôte.  Veuillez contacter le fabricant de l’appareil pour savoir s’il fournit un pilote d’atténuation des appareils.

Si vous souhaitez contourner les vérifications de sécurité pour un appareil qui n’a pas de pilote d’atténuation des appareils, vous devez transmettre le `-Force` paramètre à l' `Dismount-VMHostAssignableDevice` applet de commande.  Sachez qu’en procédant ainsi, vous avez modifié le profil de sécurité de ce système et cela est recommandé uniquement durant le prototypage ou les environnements de confiance.

## <a name="pcie-location-path"></a>Chemin de l’emplacement PCIe
Le chemin d’accès à l’emplacement PCIe est requis pour démonter et monter l’appareil à partir de l’hôte.  Un exemple de chemin d’accès à l’emplacement `"PCIROOT(20)#PCI(0300)#PCI(0000)#PCI(0800)#PCI(0000)"`ressemble à ce qui suit:.   Le [script de profil d’ordinateur](#machine-profile-script) retourne également le chemin d’accès de l’emplacement du périphérique PCIe.

### <a name="getting-the-location-path-by-using-device-manager"></a>Obtention du chemin d’accès à l’emplacement à l’aide de Device Manager
![Gestionnaire de périphériques](../deploy/media/dda-devicemanager.png)
- Ouvrez Device Manager et recherchez l’appareil.  
- Cliquez avec le bouton droit sur l’appareil et sélectionnez «Propriétés».
- Accédez à l’onglet Détails et sélectionnez «chemins d’accès des emplacements» dans la liste déroulante des propriétés.  
- Cliquez avec le bouton droit sur l’entrée qui commence par «PCIROOT» et sélectionnez «Copier».  Vous avez maintenant le chemin d’accès de l’emplacement de cet appareil.

## <a name="mmio-space"></a>Espace MMIO
Certains appareils, en particulier les GPU, requièrent l’allocation d’espace MMIO supplémentaire à la machine virtuelle pour que la mémoire de cet appareil soit accessible. Par défaut, chaque machine virtuelle démarre avec 128 Mo d’espace MMIO faible et 512 Mo d’espace MMIO élevé allouée à celle-ci. Toutefois, un appareil peut nécessiter plus d’espace MMIO, ou plusieurs appareils peuvent être transmis par le biais duquel les spécifications combinées dépassent ces valeurs.  La modification de l’espace MMIO est directe et peut être effectuée dans PowerShell à l’aide des commandes suivantes:

```PowerShell
Set-VM -LowMemoryMappedIoSpace 3Gb -VMName $vm
Set-VM -HighMemoryMappedIoSpace 33280Mb -VMName $vm
```

Le moyen le plus simple de déterminer la quantité d’espace MMIO à allouer consiste à utiliser le [script de profil d’ordinateur](#machine-profile-script). Pour télécharger et exécuter le script de profil d’ordinateur, exécutez les commandes suivantes dans une console PowerShell:

```PowerShell
curl -o SurveyDDA.ps1 https://raw.githubusercontent.com/MicrosoftDocs/Virtualization-Documentation/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1
.\SurveyDDA.ps1
```

Pour les appareils qui sont en mesure d’être affectés, le script affiche les exigences MMIO d’un appareil donné, comme dans l’exemple ci-dessous:

```PowerShell
NVIDIA GRID K520
Express Endpoint -- more secure.
    ...
    And it requires at least: 176 MB of MMIO gap space
...
```

L’espace MMIO faible est utilisé uniquement par les systèmes d’exploitation 32 bits et les appareils qui utilisent des adresses 32 bits. Dans la plupart des cas, la définition de l’espace MMIO élevé d’une machine virtuelle est suffisante, car les configurations 32 bits ne sont pas très courantes.

> [!IMPORTANT]
> Lors de l’affectation de l’espace MMIO à une machine virtuelle, l’utilisateur doit être sûr de définir l’espace MMIO sur la somme de l’espace MMIO demandé pour tous les appareils affectés souhaités, ainsi qu’une mémoire tampon supplémentaire s’il existe d’autres périphériques virtuels qui nécessitent quelques Mo d’espace MMIO. Utilisez les valeurs MMIO par défaut décrites ci-dessus comme mémoire tampon pour les valeurs MMIO faible et élevée (respectivement 128 Mo et 512 Mo).

Si un utilisateur devait affecter un seul GPU K520 comme dans l’exemple ci-dessus, il doit définir l’espace MMIO de la machine virtuelle sur la valeur générée par le script de profil d’ordinateur plus une mémoire tampon--176 Mo + 512 Mo. Si un utilisateur devait assigner trois GPU K520, il doit définir l’espace MMIO sur trois fois 176 Mo plus une mémoire tampon, ou 528 Mo + 512 Mo.

Pour plus d’informations sur l’espace MMIO, consultez attribution d' [appareil discrète-GPU](https://techcommunity.microsoft.com/t5/Virtualization/Discrete-Device-Assignment-GPUs/ba-p/382266) sur le blog TechCommunity.

## <a name="machine-profile-script"></a>Script de profil d’ordinateur
Pour simplifier l’identification si le serveur est correctement configuré et quels appareils peuvent être transmis via l’attribution discrète des appareils, l’un de nos ingénieurs a mis en place le script PowerShell suivant: [SurveyDDA. ps1.](https://github.com/Microsoft/Virtualization-Documentation/blob/live/hyperv-tools/DiscreteDeviceAssignment/SurveyDDA.ps1)

Avant d’utiliser le script, vérifiez que le rôle Hyper-V est installé et que vous exécutez le script à partir d’une fenêtre de commande PowerShell disposant de privilèges d’administrateur.

Si le système n’est pas correctement configuré pour prendre en charge l’affectation discrète des appareils, l’outil affiche un message d’erreur indiquant ce qui est incorrect. Si l’outil détecte que le système est correctement configuré, il énumère tous les périphériques qu’il peut trouver sur le bus PCIe.

Pour chaque périphérique trouvé, l’outil indique s’il peut être utilisé avec l’affectation discrète des appareils. Si un appareil est identifié comme étant compatible avec l’affectation discrète des appareils, le script vous fournira une raison.  Quand un appareil est correctement identifié comme étant compatible, le chemin d’accès à l’emplacement de l’appareil s’affiche.  En outre, si cet appareil requiert un [espace MMIO](#mmio-space), il s’affichera également.

![SurveyDDA. ps1](./images/hyper-v-surveydda-ps1.png)

## <a name="frequently-asked-questions"></a>Questions fréquemment posées

### <a name="how-does-remote-desktops-remotefx-vgpu-technology-relate-to-discrete-device-assignment"></a>Comment la technologie de processeur graphique virtuel RemoteFX de Bureau à distance est-elle liée à l’affectation discrète des appareils?
Il s’agit de technologies entièrement distinctes. Le processeur graphique virtuel RemoteFX n’a pas besoin d’être installé pour que l’attribution de périphérique discrète fonctionne. En outre, aucun rôle supplémentaire ne doit être installé. Le processeur graphique virtuel RemoteFX requiert l’installation du rôle RDVH afin que le pilote de processeur graphique virtuel RemoteFX soit présent dans la machine virtuelle. Pour l’affectation discrète des appareils, étant donné que vous allez installer le pilote du fournisseur de matériel dans la machine virtuelle, aucun autre rôle ne doit être présent.  

### <a name="ive-passed-a-gpu-into-a-vm-but-remote-desktop-or-an-application-isnt-recognizing-the-gpu"></a>J’ai passé un GPU à une machine virtuelle, mais Bureau à distance ou une application ne reconnaît pas le GPU
Cela peut se produire pour plusieurs raisons, mais plusieurs problèmes courants sont répertoriés ci-dessous.
- Assurez-vous que le dernier pilote du fournisseur de GPU est installé et qu’il ne signale pas d’erreur en vérifiant l’état de l’appareil dans la Device Manager.
- Vérifiez que l' [espace MMIO](#mmio-space) alloué à l’appareil est suffisant dans la machine virtuelle.
- Vérifiez que vous utilisez un GPU que le fournisseur prend en charge dans cette configuration. Par exemple, certains fournisseurs empêchent les cartes de leurs consommateurs de fonctionner lorsqu’ils sont passés à une machine virtuelle.
- Assurez-vous que l’application en cours d’exécution prend en charge l’exécution dans une machine virtuelle, et que le GPU et ses pilotes associés sont pris en charge par l’application. Certaines applications ont des listes d’autorisation des GPU et des environnements.
- Si vous utilisez le rôle d’hôte de session Bureau à distance ou Windows multipoint services sur l’invité, vous devez vous assurer qu’une entrée de stratégie de groupe spécifique est définie pour autoriser l’utilisation du GPU par défaut. À l’aide d’un objet stratégie de groupe appliqué à l’invité (ou à l’éditeur de stratégie de groupe local sur l’invité), accédez à l’élément de stratégie de groupe suivant:
   - Configuration ordinateur
   - Modèles d’administrateur
   - Composants Windows
   - Services Bureau à distance
   - Hôte de session Bureau à distance
   - Environnement de session à distance
   - Utiliser la carte graphique par défaut matérielle pour toutes les sessions Services Bureau à distance

    Définissez cette valeur sur activé, puis redémarrez la machine virtuelle une fois que la stratégie a été appliquée.

### <a name="can-discrete-device-assignment-take-advantage-of-remote-desktops-avc444-codec"></a>L’attribution d’appareil discrète peut-elle tirer parti du codec AVC444 de Bureau à distance?
Oui, consultez ce billet de blog pour plus d’informations: [Améliorations de l’protocole RDP (Remote Desktop Protocol) (RDP) 10 AVC/H. 264 dans Windows 10 et Windows Server 2016 Technical Preview.](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

### <a name="can-i-use-powershell-to-get-the-location-path"></a>Puis-je utiliser PowerShell pour récupérer le chemin d’accès à l’emplacement?
Oui, il existe différentes façons de procéder. Voici un exemple:
```
#Enumerate all PNP Devices on the system
$pnpdevs = Get-PnpDevice -presentOnly
#Select only those devices that are Display devices manufactured by NVIDIA
$gpudevs = $pnpdevs |where-object {$_.Class -like "Display" -and $_.Manufacturer -like "NVIDIA"}
#Select the location path of the first device that's available to be dismounted by the host.
$locationPath = ($gpudevs | Get-PnpDeviceProperty DEVPKEY_Device_LocationPaths).data[0]
```

### <a name="can-discrete-device-assignment-be-used-to-pass-a-usb-device-into-a-vm"></a>Est-il possible d’utiliser l’attribution d’appareil discrète pour passer un périphérique USB à une machine virtuelle?
Bien qu’ils ne soient pas officiellement pris en charge, nos clients ont utilisé l’affectation discrète des appareils pour effectuer cette opération en transmettant l’ensemble du contrôleur USB3 dans une machine virtuelle.  Lorsque l’ensemble du contrôleur est passé, chaque périphérique USB relié à ce contrôleur est également accessible dans la machine virtuelle.  Notez que seuls certains contrôleurs USB3 peuvent fonctionner et que les contrôleurs USB2 ne peuvent pas être utilisés avec l’affectation discrète des appareils.

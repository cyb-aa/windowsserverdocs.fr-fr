---
title: Configurer et afficher les paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V
description: Vous pouvez utiliser cette rubrique pour découvrir les meilleures pratiques pour la configuration et l’affichage des paramètres de réseau local virtuel (VLAN) virtuel sur un port de commutateur virtuel Hyper-V dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e4843b0ffee86d728736ae212b953bb7c8552c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820550"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configurer et afficher les paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour découvrir les meilleures pratiques pour la configuration et l’affichage des paramètres de réseau local virtuel (VLAN) virtuel sur un port de commutateur virtuel Hyper-V.

Lorsque vous souhaitez configurer les paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V, vous pouvez utiliser soit Windows&reg; Server 2016 Hyper-V Manager ou System Center Virtual Machine Manager (VMM).

Si vous utilisez VMM, VMM utilise la commande Windows PowerShell suivante pour configurer le port de commutateur.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Si vous n’utilisez pas de VMM et configurez le port de commutateur dans Windows Server, vous pouvez utiliser la console du Gestionnaire Hyper-V ou la commande Windows PowerShell suivante.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

En raison de ces deux méthodes de configuration des paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V, il est possible que lorsque vous tentez d’afficher les paramètres de port de commutateur, il s’affiche pour vous que les paramètres de réseau local virtuel sont non configurés - même lorsqu’elles sont configurées.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Utiliser la même méthode pour configurer et afficher les paramètres de réseau local virtuel de port de commutateur

Pour vous assurer que vous ne rencontrez pas ces problèmes, vous devez utiliser la même méthode pour afficher vos paramètres de réseau local virtuel de port de commutateur que vous avez utilisé pour configurer les paramètres de réseau local virtuel de port de commutateur.

Pour configurer et afficher les paramètres de port de commutateur de réseau local virtuel, vous devez procédez comme suit :

- Si vous utilisez VMM ou contrôleur de réseau pour configurer et gérer votre réseau et que vous avez déployé la mise en réseau SDN (Software Defined), vous devez utiliser le **VMNetworkAdapterIsolation** applets de commande. 
- Si vous utilisez les applets de commande Windows Server 2016 Hyper-V Manager ou Windows PowerShell, et vous n’avez pas déployé de mise en réseau SDN (Software Defined), vous devez utiliser le **VMNetworkAdapterVlan** applets de commande.

### <a name="possible-issues"></a>Problèmes possibles

Si vous ne suivez pas ces instructions, vous pouvez rencontrer les problèmes suivants.

- Dans les cas où vous avez déployé SDN et que vous utilisez VMM, contrôleur de réseau, ou le **VMNetworkAdapterIsolation** applets de commande pour configurer les paramètres de réseau local virtuel sur un port de commutateur virtuel Hyper-V : Si vous utilisez le Gestionnaire Hyper-V ou **VMNetworkAdapterVlan obtenir** pour afficher les paramètres de configuration, la sortie de commande n’affichera pas vos paramètres de réseau local virtuel. Au lieu de cela, vous devez utiliser le **Get-VMNetworkIsolation** applet de commande pour afficher les paramètres de réseau local virtuel.
- Dans les cas où vous n’avez pas déployé SDN et utilisez plutôt le Gestionnaire Hyper-V ou le **VMNetworkAdapterVlan** applets de commande pour configurer les paramètres de réseau local virtuel sur un port de commutateur virtuel Hyper-V : Si vous utilisez le **Get-VMNetworkIsolation** applet de commande pour afficher les paramètres de configuration, la sortie de commande n’affichera pas vos paramètres de réseau local virtuel. Au lieu de cela, vous devez utiliser le **VMNetworkAdapterVlan obtenir** applet de commande pour afficher les paramètres de réseau local virtuel.

Il est également important de ne pas tenter de configurer les mêmes paramètres de réseau local virtuel de port de commutateur à l’aide de ces méthodes de configuration. Si vous procédez ainsi, le port de commutateur est configuré correctement, et le résultat peut être un échec de communication réseau.

### <a name="resources"></a>Ressources

Pour plus d’informations sur les commandes Windows PowerShell qui sont mentionnées dans cette rubrique, consultez les rubriques suivantes :

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [Get-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [Get-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)






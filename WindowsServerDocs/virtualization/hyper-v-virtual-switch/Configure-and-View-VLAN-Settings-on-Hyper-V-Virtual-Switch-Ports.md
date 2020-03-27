---
title: Configurer et afficher les paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V
description: Vous pouvez utiliser cette rubrique pour découvrir les meilleures pratiques pour la configuration et l’affichage des paramètres de réseau local virtuel (VLAN) sur un port de commutateur virtuel Hyper-V dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 69e0e28a-98ae-4ade-bd27-ce2ad7eb310f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 083558762051283115211d10d32ebb6fd3ad3953
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308034"
---
# <a name="configure-and-view-vlan-settings-on-hyper-v-virtual-switch-ports"></a>Configurer et afficher les paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour découvrir les meilleures pratiques pour la configuration et l’affichage des paramètres de réseau local virtuel (VLAN) sur un port de commutateur virtuel Hyper-V.

Si vous souhaitez configurer les paramètres de réseau local virtuel sur des ports de commutateur virtuel Hyper-V, vous pouvez utiliser le Gestionnaire Hyper-V ou le System Center Virtual Machine Manager (VMM) Windows&reg; Server 2016.

Si vous utilisez VMM, VMM utilise la commande Windows PowerShell suivante pour configurer le port commuté.

```
Set-VMNetworkAdapterIsolation <VM-name|-managementOS> -IsolationMode VLAN -DefaultIsolationID <vlan-value> -AllowUntaggedTraffic $True
```
Si vous n’utilisez pas VMM et que vous configurez le port de commutateur dans Windows Server, vous pouvez utiliser la console du Gestionnaire Hyper-V ou la commande Windows PowerShell suivante.
```
Set-VMNetworkAdapterVlan <VM-name|-managementOS> -Access -VlanID <vlan-value>
```

En raison de ces deux méthodes de configuration des paramètres de réseau local virtuel sur les ports de commutateur virtuel Hyper-V, il est possible que lorsque vous tentez d’afficher les paramètres de port du commutateur, les paramètres du réseau local virtuel ne sont pas configurés, même s’ils sont configurés.

## <a name="use-the-same-method-to-configure-and-view-switch-port-vlan-settings"></a>Utilisez la même méthode pour configurer et afficher les paramètres de réseau local virtuel du port commuté

Pour vous assurer que vous ne rencontrez pas ces problèmes, vous devez utiliser la même méthode pour afficher les paramètres de réseau local virtuel du port de commutateur que vous avez utilisés pour configurer les paramètres de réseau local virtuel du port commuté.

Pour configurer et afficher les paramètres de port du commutateur de réseau local virtuel, vous devez effectuer les opérations suivantes :

- Si vous utilisez VMM ou le contrôleur de réseau pour configurer et gérer votre réseau et que vous avez déployé SDN (Software Defined Networking), vous devez utiliser les applets de commande **VMNetworkAdapterIsolation** . 
- Si vous utilisez le Gestionnaire Hyper-V Windows Server 2016 ou des applets de commande Windows PowerShell et que vous n’avez pas déployé le réseau à définition logicielle (SDN), vous devez utiliser les applets de commande **VMNetworkAdapterVlan** .

### <a name="possible-issues"></a>Problèmes possibles

Si vous ne suivez pas ces instructions, vous pouvez rencontrer les problèmes suivants.

- Dans les cas où vous avez déployé SDN et utilisé VMM, le contrôleur de réseau ou les applets de commande **VMNetworkAdapterIsolation** pour configurer les paramètres de réseau local virtuel sur un port de commutateur virtuel Hyper-v : Si vous utilisez le Gestionnaire Hyper-v ou **VMNetworkAdapterVlan** pour afficher les paramètres de configuration, la sortie de la commande n’affiche pas vos paramètres de réseau local virtuel. Au lieu de cela, vous devez utiliser l’applet de commande **VMNetworkIsolation** pour afficher les paramètres de réseau local virtuel.
- Dans les cas où vous n’avez pas déployé SDN et utilisez à la place le Gestionnaire Hyper-V ou les applets de commande **VMNetworkAdapterVlan** pour configurer les paramètres de réseau local virtuel sur un port de commutateur virtuel Hyper-v : Si vous utilisez l’applet de commande **VMNetworkIsolation** pour afficher les paramètres de configuration, la sortie de la commande n’affiche pas vos paramètres de réseau local virtuel. Au lieu de cela, vous devez utiliser l’applet de commande **obtenir VMNetworkAdapterVlan** pour afficher les paramètres de réseau local virtuel.

Il est également important de ne pas tenter de configurer les mêmes paramètres de VLAN de port de commutateur à l’aide de ces deux méthodes de configuration. Si vous procédez ainsi, le port de commutateur est incorrectement configuré et le résultat peut être un échec dans la communication réseau.

### <a name="resources"></a>Ressources

Pour plus d’informations sur les commandes Windows PowerShell mentionnées dans cette rubrique, consultez les rubriques suivantes :

- [Set-VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464283.aspx)
- [VmNetworkAdapterIsolation](https://technet.microsoft.com/library/dn464277.aspx)
- [Set-VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848475.aspx)
- [VMNetworkAdapterVlan](https://technet.microsoft.com/library/hh848516.aspx)






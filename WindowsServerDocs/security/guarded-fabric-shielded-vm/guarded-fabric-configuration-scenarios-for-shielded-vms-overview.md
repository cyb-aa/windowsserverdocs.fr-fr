---
title: Déployer des machines virtuelles protégées
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 9badbfcb709c29451425aaecc56b46ac98837e18
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443802"
---
# <a name="deploy-shielded-vms"></a>Déployer des machines virtuelles protégées


>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les rubriques suivantes décrivent comment un client peut fonctionner avec des machines virtuelles protégées.

1. (Facultatif) [Créer un modèle de disque Windows](guarded-fabric-create-a-shielded-vm-template.md) ou [créer un modèle de disque Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). Le disque de modèle peut être créé par le client ou le fournisseur de services. 

2. (Facultatif) [Convertir une machine virtuelle Windows existante à une machine virtuelle protégée](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Créer des données de protection pour définir une machine virtuelle protégée](guarded-fabric-tenant-creates-shielding-data.md).

    Pour une description et un diagramme d’un fichier de données de protection, consultez [ce qui est des données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Pour plus d’informations sur la création d’un fichier de réponses à inclure dans un fichier de données protégées, consultez [machines virtuelles protégées - générer un fichier de réponses à l’aide de la fonction New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Créer une machine virtuelle protégée :
 
    - À l’aide de **Windows Azure Pack**: [Déployer une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - À l’aide de **de Virtual Machine Manager**: [Déployer une machine virtuelle protégée à l’aide de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Créer un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Voir aussi

- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)

---
title: Déployer des machines virtuelles protégées
ms.prod: windows-server
ms.topic: article
ms.assetid: 5d1a06c9-24e1-4e14-9c9a-efb2adbfeddd
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: f4550f8a92330c8f483e332ab9e4b36fda853b0a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856862"
---
# <a name="deploy-shielded-vms"></a>Déployer des machines virtuelles protégées


>S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), Windows Server 2016

Les rubriques suivantes décrivent comment un locataire peut travailler avec des machines virtuelles protégées.

1. Facultatif [Créez un disque de modèle Windows](guarded-fabric-create-a-shielded-vm-template.md) ou [créez un disque de modèle Linux](guarded-fabric-create-a-linux-shielded-vm-template.md). Le disque de modèle peut être créé par le client ou le fournisseur de services d’hébergement. 

2. Facultatif [Conversion d’une machine virtuelle Windows existante en machine virtuelle protégée](guarded-fabric-vm-shielding-helper-vhd.md). 

3. [Créer des données de protection pour définir une machine virtuelle protégée](guarded-fabric-tenant-creates-shielding-data.md).

    Pour obtenir une description et un diagramme d’un fichier de données de protection, consultez [qu’est-ce que les données de protection et pourquoi est-il nécessaire ?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary)
    
    Pour plus d’informations sur la création d’un fichier de réponses à inclure dans un fichier de données protégé, consultez [machines virtuelles protégées-générer un fichier de réponses à l’aide de la fonction New-ShieldingDataAnswerFile](guarded-fabric-sample-unattend-xml-file.md).

4. Créer une machine virtuelle protégée :
 
    - Utilisation de **Windows Azure Pack**: [déployer une machine virtuelle protégée à l’aide de Windows Azure Pack](guarded-fabric-shielded-vm-windows-azure-pack.md)

    - Utilisation de **Virtual Machine Manager**: [déployer une machine virtuelle protégée à l’aide de Virtual Machine Manager](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)

## <a name="next-step"></a>Étape suivante

> [!div class="nextstepaction"]
> [Créer un modèle de machine virtuelle protégée](guarded-fabric-create-a-shielded-vm-template.md)

## <a name="see-also"></a>Voir aussi

- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)

---
title: Machines virtuelles protégées pour les locataires-déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: ec9f12990e7e16aebb208edfe0d97d6671623da1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403537"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Machines virtuelles protégées pour les locataires-déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2019, Windows Server 2016

Si votre fournisseur de services d’hébergement le prend en charge, vous pouvez utiliser Windows Azure Pack pour déployer une machine virtuelle protégée.

Procédez comme suit:

1. Abonnez-vous à un ou plusieurs plans proposés dans Windows Azure Pack.

2. Créez une machine virtuelle protégée à l’aide de Windows Azure Pack.

    [Utilisez des machines virtuelles protégées](https://technet.microsoft.com/library/mt720674.aspx), qui sont décrites dans les rubriques suivantes :

   - [Créez des données de protection](https://technet.microsoft.com/library/mt720672.aspx) (et chargez le fichier de données de protection, comme décrit dans la deuxième procédure de la rubrique).
    
     > [!NOTE]
     > Dans le cadre de la création de données de protection, vous allez télécharger votre fichier de clé Guardian, qui sera un fichier XML au format UTF-8. Ne modifiez pas le fichier en UTF-16.
    
   - [Créez une machine virtuelle protégée](https://technet.microsoft.com/library/mt720673.aspx) , avec **création rapide**, via un modèle protégé ou un modèle standard.
    
       > [!WARNING]
       > Si vous [créez une machine virtuelle protégée à l’aide d’un modèle standard](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), il est important de noter que la machine virtuelle est configurée comme non *protégée*. Cela signifie que le disque de modèle n’est pas vérifié par rapport à la liste des disques approuvés dans votre fichier de données de protection, ni des secrets dans votre fichier de données de protection utilisé pour approvisionner la machine virtuelle. Si un modèle protégé est disponible, il est préférable de déployer une machine virtuelle protégée avec un modèle protégé pour fournir une protection de bout en bout de vos secrets.
    
   - [Convertir un ordinateur virtuel de génération 2 en ordinateur virtuel protégé](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > Si vous convertissez un ordinateur virtuel en ordinateur virtuel protégé, les sauvegardes et les points de contrôle existants ne sont pas chiffrés. Lorsque cela est possible, vous devez supprimer les anciens points de contrôle pour empêcher l’accès à vos anciennes données déchiffrées.

## <a name="see-also"></a>Voir aussi

- [Étapes de configuration du fournisseur de services d’hébergement pour les hôtes service Guardian et les machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)

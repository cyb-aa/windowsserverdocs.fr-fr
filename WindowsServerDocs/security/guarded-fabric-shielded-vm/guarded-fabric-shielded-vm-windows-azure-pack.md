---
title: Machines virtuelles protégées pour les locataires - déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 095315e4-c4a7-4b80-91d8-528119b62c4c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 600ccd74c379daa281f438b1200179dcae210817
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447350"
---
# <a name="shielded-vms--for-tenants---deploying-a-shielded-vm-by-using-windows-azure-pack"></a>Machines virtuelles protégées pour les locataires - déploiement d’une machine virtuelle protégée à l’aide de Windows Azure Pack

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2019, Windows Server 2016

Si votre fournisseur de services prend en charge, vous pouvez utiliser Windows Azure Pack pour déployer une machine virtuelle protégée.

Procédez comme suit :

<!-- When we have a link to the topic about how tenants subscribe, add that link as an indented item just under step 1 below. -->

1. S’abonner à un ou plusieurs plans proposés dans Windows Azure Pack.

2. Créer une machine virtuelle protégée à l’aide de Windows Azure Pack.

    [Utilisez des machines virtuelles protégées](https://technet.microsoft.com/library/mt720674.aspx), qui est décrite dans les rubriques suivantes :

   - [Créer des données de protection](https://technet.microsoft.com/library/mt720672.aspx) (et chargez le fichier de données de protection, comme décrit dans la deuxième procédure dans la rubrique).
    
     > [!NOTE]
     > Dans le cadre de la création des données de protection, vous allez télécharger votre fichier de clé de gardien, qui sera un fichier XML au format UTF-8. Ne modifiez pas le fichier UTF-16.
    
   - [Créer une machine virtuelle protégée](https://technet.microsoft.com/library/mt720673.aspx) - avec **création rapide**, via un modèle protégé ou via un modèle Normal.
    
       > [!WARNING]
       > Si vous [créer une machine virtuelle protégée à l’aide d’un modèle standard](https://technet.microsoft.com/library/mt720673.aspx#Anchor_2), il est important de noter que la machine virtuelle est configurée *non blindés*. Cela signifie que le disque de modèle n’est pas vérifié par rapport à la liste des disques approuvés dans votre fichier de données de protection, ni les clés secrètes dans votre fichier de données de protection permettent de configurer la machine virtuelle. Si un modèle protégé est disponible, il est préférable de déployer une machine virtuelle protégée avec un modèle protégé pour assurer la protection de bout en bout de vos secrets.
    
   - [Convertir une machine virtuelle de génération 2 à une machine virtuelle protégée](https://technet.microsoft.com/library/mt720670.aspx)
    
       > [!NOTE]
       > Si vous convertissez un ordinateur virtuel à une machine virtuelle protégée, les sauvegardes et les points de contrôle existants ne sont pas chiffrés. Vous devez supprimer les anciens points de contrôle lorsque cela est possible d’empêcher l’accès à vos anciennes données déchiffrées.

## <a name="see-also"></a>Voir aussi

- [Hébergement des étapes de configuration de fournisseur de service pour les hôtes service Guardian et des machines virtuelles protégées](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Structure protégée et machines virtuelles dotées d’une protection maximale](guarded-fabric-and-shielded-vms-top-node.md)

---
title: Considérations sur les succursales
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: 93bf1f8993827ab737c95abad1335317d4e9b599
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70870445"
---
# <a name="branch-office-considerations"></a>Éléments à prendre en compte en matière de filiale

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), 

Cet article décrit les meilleures pratiques pour l’exécution de machines virtuelles protégées dans les succursales et dans d’autres scénarios distants où les hôtes Hyper-V peuvent avoir des périodes avec une connectivité limitée à SGH.

## <a name="fallback-configuration"></a>Configuration de secours

À compter de la version 1709 de Windows Server, vous pouvez configurer un ensemble supplémentaire d’URL de service Guardian hôte sur les hôtes Hyper-V à utiliser lorsque le SGH principal ne répond pas.
Cela vous permet d’exécuter un cluster SGH local qui est utilisé en tant que serveur principal pour de meilleures performances avec la possibilité de revenir au SGH de votre centre de résultats d’entreprise si les serveurs locaux sont en défaillance.

Pour utiliser l’option de secours, vous devez configurer deux serveurs SGH. Ils peuvent exécuter Windows Server 2019 ou Windows Server 2016 et faire partie de clusters identiques ou différents. S’il s’agit de clusters différents, vous pouvez établir des pratiques opérationnelles pour vous assurer que les stratégies d’attestation sont synchronisées entre les deux serveurs. Ils doivent tous deux être en mesure d’autoriser correctement l’hôte Hyper-V à exécuter des machines virtuelles dotées d’une protection maximale et de disposer du matériel de clé nécessaire pour démarrer les machines virtuelles protégées. Vous pouvez choisir d’avoir une paire de certificats de chiffrement partagés et de certificats de signature entre les deux clusters, ou utiliser des certificats distincts et configurer la machine virtuelle dotée d’une protection maximale pour autoriser les deux gardiens (paires de certificats de chiffrement/signature) dans les données de protection. txt.

Ensuite, mettez à niveau vos ordinateurs hôtes Hyper-V vers Windows Server version 1709 ou Windows Server 2019, puis exécutez la commande suivante :
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Pour annuler la configuration d’un serveur de secours, il suffit d’omettre les deux paramètres de secours :
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Pour que l’hôte Hyper-V réussisse l’attestation avec les serveurs principal et de secours, vous devez vous assurer que les informations d’attestation sont à jour avec les deux clusters SGH.
En outre, les certificats utilisés pour déchiffrer le module de plateforme sécurisée de l’ordinateur virtuel doivent être disponibles dans les deux clusters SGH.
Vous pouvez configurer chaque SGH avec des certificats différents et configurer la machine virtuelle pour qu’elle approuve les deux, ou ajouter un ensemble partagé de certificats aux deux clusters SGH.

Pour plus d’informations sur la configuration de SGH dans une filiale à l’aide d’URL de secours, consultez le billet de blog [amélioration de la prise en charge des filiales pour les machines virtuelles protégées dans Windows Server, version 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>Mode hors connexion

Le mode hors connexion permet à votre machine virtuelle protégée de s’activer quand SGH est inaccessible, tant que la configuration de sécurité de votre hôte Hyper-V n’a pas changé.
Le mode hors connexion fonctionne en mettant en cache une version spéciale du protecteur de clé TPM de machine virtuelle sur l’hôte Hyper-V.
Le protecteur de clé est chiffré à la configuration de sécurité actuelle de l’hôte (à l’aide de la clé d’identité de sécurité basée sur la virtualisation).
Si votre hôte ne peut pas communiquer avec SGH et que sa configuration de sécurité n’a pas changé, il peut utiliser le protecteur de clé mis en cache pour démarrer la machine virtuelle protégée.
Lorsque les paramètres de sécurité changent sur le système, par exemple si une nouvelle stratégie d’intégrité du code est appliquée ou si le démarrage sécurisé est désactivé, les protecteurs de clés mis en cache sont invalidés et l’hôte doit attester avec un SGH pour que les machines virtuelles protégées puissent être redémarrées hors connexion.

Le mode hors connexion nécessite Windows Server Insider preview version 17609 ou ultérieure pour le cluster du service Guardian hôte et l’hôte Hyper-V.
Elle est contrôlée par une stratégie sur SGH, qui est désactivée par défaut.
Pour activer la prise en charge du mode hors connexion, exécutez la commande suivante sur un nœud SGH :

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Étant donné que les protecteurs de clés pouvant être mis en cache sont uniques à chaque machine virtuelle protégée, vous devez arrêter complètement (sans redémarrer) et démarrer vos machines virtuelles protégées pour obtenir un protecteur de clé pouvant être mis en cache une fois ce paramètre activé sur SGH.
Si votre machine virtuelle protégée est migrée vers un hôte Hyper-V exécutant une version antérieure de Windows Server, ou si vous obtenez un nouveau protecteur de clé à partir d’une version antérieure de SGH, il ne pourra pas démarrer lui-même en mode hors connexion, mais peut continuer à s’exécuter en mode en ligne quand l’accès à SGH est disponible. se.

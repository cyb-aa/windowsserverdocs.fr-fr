---
title: Considérations relatives à la branche Office
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: d93c37227af1eb62368fbcd4ec5d6a48374b45ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877060"
---
# <a name="branch-office-considerations"></a>Éléments à prendre en compte en matière de filiale

> S’applique à : Windows Server 2019, Windows Server (canal semi-annuel), 

Cet article décrit les meilleures pratiques pour l’exécution des machines virtuelles protégées dans les succursales et autres scénarios distants où les ordinateurs hôtes Hyper-V peuvent avoir des périodes de temps avec une connectivité limitée à SGH.

## <a name="fallback-configuration"></a>Configuration de secours

À compter de Windows Server version 1709, vous pouvez configurer un jeu supplémentaire de l’URL du Service Guardian hôte sur des ordinateurs hôtes Hyper-V pour une utilisation lorsque votre service HGS principal ne répond pas.
Cela vous permet de vous permettent d’exécuter un cluster SGH local qui est utilisé comme un serveur principal pour de meilleures performances avec la possibilité de revenir au service SGH de votre centre de données d’entreprise, si les serveurs locaux sont arrêtés.

Pour utiliser l’option de secours, vous devez configurer deux serveurs SGH. Ils peuvent exécuter Windows Server 2019 ou Windows Server 2016 et soit faire partie des clusters identiques ou différents. S’ils sont des clusters différents, vous devez établir des pratiques opérationnelles afin des stratégies d’attestation sont synchronisés entre les deux serveurs. Les deux doivent être en mesure d’autoriser correctement l’hôte Hyper-V pour exécuter des machines virtuelles protégées et permettre le matériel de clé nécessaire pour démarrer les machines virtuelles protégées. Vous pouvez choisir d’avoir une paire de chiffrement partagée et de la signature des certificats entre les deux clusters, ou utiliser des certificats distincts et pour configurer le service SGH protégées de machine virtuelle pour autoriser les deux gardiens (paires de certificat de signature/chiffrement) dans les données de protection fichier.

Mise à niveau de vos hôtes Hyper-V pour Windows Server version 1709 ou Windows Server 2019, puis exécutez la commande suivante :
```powershell
# Replace https://hgs.primary.com and https://hgs.backup.com with your own domain names and protocols
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation' -FallbackKeyProtectionServerUrl 'https://hgs.backup.com/KeyProtection' -FallbackAttestationServerUrl 'https://hgs.backup.com/Attestation'
```

Pour annuler la configuration d’un serveur de secours, omettez simplement les deux paramètres de secours :
```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'https://hgs.primary.com/KeyProtection' -AttestationServerUrl 'https://hgs.primary.com/Attestation'
```

Dans l’ordre pour l’hôte Hyper-V passer une attestation avec les serveurs principales et de secours, vous devez vous assurer que vos informations d’attestation sont à jour avec les deux clusters SGH.
En outre, les certificats utilisés pour déchiffrer le module de plateforme sécurisée de la machine virtuelle doivent être disponibles dans les deux clusters SGH.
Vous pouvez configurer chaque SGH avec différents certificats et configurer la machine virtuelle pour faire confiance à la fois ou ajouter un ensemble partagé de certificats pour les deux clusters SGH.

Pour plus d’informations sur la configuration de SGH dans une succursale à l’aide des URL de secours, consultez le blog [amélioré de support des succursales pour les machines virtuelles protégées dans Windows Server, version 1709](https://blogs.technet.microsoft.com/datacentersecurity/2017/11/15/improved-branch-office-support-for-shielded-vms-in-windows-server-version-1709/).


## <a name="offline-mode"></a>En mode hors connexion

En mode hors connexion permet à votre machine virtuelle protégée à activer lorsque SGH ne peut pas être atteinte, tant que la configuration de sécurité de votre hôte Hyper-V n’a pas changé.
En mode hors connexion fonctionne en mettant en cache d’une version spéciale du protecteur de clé VM TPM sur l’ordinateur hôte Hyper-V.
Le protecteur de clé est chiffré à la configuration actuelle de la sécurité de l’hôte (à l’aide de la clé d’identité de sécurité en fonction de virtualisation).
Si votre hôte ne peut pas communiquer avec SGH et sa configuration de sécurité n’a pas changé, il sera en mesure d’utiliser le protecteur de clé mise en cache pour démarrer la machine virtuelle protégée.
Si les paramètres de sécurité changent sur le système, tel qu’un nouvelle stratégie d’intégrité du code en cours d’application ou le démarrage sécurisé est désactivé, les protecteurs de clé mise en cache seront invalidées et l’hôte disposera d’attester avec un service SGH avant tout des machines virtuelles protégées peuvent être démarrées en mode hors connexion à nouveau.

En mode hors connexion nécessite une build de Windows Server Insider Preview 17609 ou une version ultérieure pour le cluster du Service Guardian hôte et l’hôte Hyper-V.
Elle est contrôlée par une stratégie sur SGH, qui est désactivé par défaut.
Pour activer la prise en charge pour le mode hors connexion, exécutez la commande suivante sur un nœud SGH :

```powershell
Set-HgsKeyProtectionConfiguration -AllowKeyMaterialCaching:$true
```

Dans la mesure où les protecteurs de clé mis en cache sont uniques à chaque machine virtuelle protégée, vous devez entièrement arrêter (pas de redémarrage) et démarrent à vos machines virtuelles protégées pour obtenir un protecteur de clé mis en cache une fois ce paramètre est activé sur SGH.
Si votre machine virtuelle protégée migre vers un hôte Hyper-V exécutant une version antérieure de Windows Server, ou obtient un nouveau protecteur de clé à partir d’une version antérieure de SGH, il ne sera pas en mesure de lui-même démarrent en mode hors connexion, mais il peut poursuivre son exécution en mode en ligne lorsque l’accès à SGH est disponible possibilité.

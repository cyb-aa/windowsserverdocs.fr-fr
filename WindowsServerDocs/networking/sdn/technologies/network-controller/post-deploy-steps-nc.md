---
title: Étapes de post-déploiement pour contrôleur de réseau
description: Cette rubrique fournit des instructions de configuration de certificat pour les déploiements non-Kerberos du contrôleur de réseau dans Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3f95d2884a808239c1d171eecbc983e26e799102
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71355616"
---
# <a name="post-deployment-steps-for-network-controller"></a>Étapes de post-déploiement pour contrôleur de réseau

Lorsque vous installez le contrôleur de réseau, vous pouvez choisir des déploiements Kerberos ou non-Kerberos.

Pour les déploiements Kerberos non\-, vous devez configurer des certificats.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurer des certificats pour les déploiements non-Kerberos

Si les ordinateurs ou les ordinateurs virtuels \(les machines virtuelles\) pour le contrôleur de réseau et le client de gestion ne sont pas joints au domaine\-, vous devez configurer l’authentification basée sur le certificat\-en effectuant les étapes suivantes.

- Créez un certificat sur le contrôleur de réseau pour l’authentification de l’ordinateur. Le nom d’objet du certificat doit être le même que le nom DNS de l’ordinateur du contrôleur de réseau ou de la machine virtuelle.

- Créez un certificat sur le client de gestion. Ce certificat doit être approuvé par le contrôleur de réseau.
  
- Inscrire un certificat sur l’ordinateur ou la machine virtuelle du contrôleur de réseau. Le certificat doit remplir les conditions suivantes.
  
    -  L’objectif de l’authentification du serveur et l’authentification du client doivent être configurés dans l’utilisation améliorée de la clé \(\) EKU ou les extensions des stratégies d’application. L’identificateur d’objet pour l’authentification du serveur est 1.3.6.1.5.5.7.3.1. L’identificateur d’objet pour l’authentification du client est 1.3.6.1.5.5.7.3.2.
  
    - Le nom d’objet du certificat doit être résolu en :
  
        - L’adresse IP de l’ordinateur ou de la machine virtuelle du contrôleur de réseau, si le contrôleur de réseau est déployé sur un ordinateur ou une machine virtuelle unique.

        - L’adresse IP REST, si le contrôleur de réseau est déployé sur plusieurs ordinateurs, plusieurs machines virtuelles ou les deux.
  
    - Ce certificat doit être approuvé par tous les clients REST. Le certificat doit également être approuvé par le multiplexeur de l’équilibrage de charge logiciel (SLB) et les ordinateurs hôtes Southbound gérés par le contrôleur de réseau.
  
    - Le certificat peut être inscrit par une autorité de certification (CA) ou être un certificat auto-signé. Les certificats auto-signés ne sont pas recommandés pour les déploiements de production, mais sont acceptables pour les environnements de laboratoire de test.
  
    - Le même certificat doit être approvisionné sur tous les nœuds du contrôleur de réseau. Après avoir créé le certificat sur un nœud, vous pouvez exporter le certificat (avec une clé privée) et l’importer sur les autres nœuds.

Pour plus d’informations, voir [Contrôleur de réseau](Network-Controller.md).

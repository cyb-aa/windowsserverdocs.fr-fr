---
title: Étapes de post-déploiement pour contrôleur de réseau
description: Cette rubrique fournit des instructions de configuration de certificat pour les déploiements non Kerberos du contrôleur de réseau dans Windows Server 2016 Datacenter.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: eea0aca9-8d89-48fb-8068-fca40c90d34b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f7d6bbd50537e24f392eabde7d103c91a4f07c90
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871080"
---
# <a name="post-deployment-steps-for-network-controller"></a>Étapes de post-déploiement pour contrôleur de réseau

Lorsque vous installez le contrôleur de réseau, vous pouvez choisir des déploiements Kerberos ou non Kerberos.

Pour non\-déploiements Kerberos, vous devez configurer des certificats.

## <a name="configure-certificates-for-non-kerberos-deployments"></a>Configurer des certificats pour les déploiements non-Kerberos

Si les ordinateurs ou les machines virtuelles \(machines virtuelles\) pour contrôleur de réseau et le client de gestion ne sont pas domaine\-joint, vous devez configurer certificat\-en fonction de l’authentification en effectuant ce qui suit étapes.

- Créer un certificat sur le contrôleur de réseau pour l’authentification d’ordinateur. Le nom de sujet du certificat doit être identique au nom DNS de l’ordinateur de contrôleur de réseau ou d’une machine virtuelle.

- Créer un certificat sur le client de gestion. Ce certificat doit être approuvé par le contrôleur de réseau.
  
- Inscrire un certificat sur l’ordinateur de contrôleur de réseau ou d’une machine virtuelle. Le certificat doit remplir les conditions suivantes.
  
    -  L’objectif de l’authentification du serveur et l’objectif de l’authentification du Client doivent être configurés dans Enhanced Key Usage \(EKU\) ou les extensions de stratégies d’Application. L’identificateur d’objet pour l’authentification du serveur est 1.3.6.1.5.5.7.3.1. L’identificateur d’objet pour l’authentification du Client est 1.3.6.1.5.5.7.3.2.
  
    - Le nom de sujet du certificat doit se résoudre :
  
        - L’adresse IP de l’ordinateur de contrôleur de réseau ou d’une machine virtuelle, si le contrôleur de réseau est déployé sur un seul ordinateur ou d’une machine virtuelle.

        - L’adresse IP REST, si le contrôleur de réseau est déployé sur plusieurs ordinateurs, plusieurs machines virtuelles ou les deux.
  
    - Ce certificat doit être approuvé par tous les clients REST. Le certificat doit également être approuvé par le multiplexeur d’équilibrage de charge logiciel (SLB) (MUX) et les ordinateurs hôtes southbound qui sont gérés par le contrôleur de réseau.
  
    - Le certificat peut être inscrit par une autorité de Certification (CA) ou peut être un certificat auto-signé. Les certificats auto-signés ne sont pas recommandés pour les déploiements de production, mais sont acceptables pour les environnements de laboratoire de test.
  
    - Le même certificat doit être configuré sur tous les nœuds de contrôleur de réseau. Après avoir créé le certificat sur un nœud, vous pouvez exporter le certificat (avec une clé privée) et importez-le sur les autres nœuds.

Pour plus d’informations, voir [Contrôleur de réseau](Network-Controller.md).

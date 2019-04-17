---
title: Sécurité du contrôleur de réseau
description: Vous pouvez utiliser cette rubrique pour savoir comment configurer la sécurité pour toutes les communications entre le contrôleur de réseau et autres logiciels et les appareils.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ab04d3f528beb037988e6390fe85ad9af219266b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller-security"></a>Sécurité du contrôleur de réseau

Vous pouvez utiliser cette rubrique pour savoir comment configurer la sécurité pour toutes les communications entre le contrôleur de réseau et autres logiciels et les appareils. 

>[!NOTE]
>Pour une vue d’ensemble du contrôleur de réseau, voir [contrôleur de réseau](../technologies/network-controller/network-controller.md).

Les chemins de communication que vous pouvez sécuriser incluent une communication Northbound sur le plan de gestion, les communications de cluster entre le contrôleur de réseau des ordinateurs virtuels \(VMs\) dans un cluster et la communication Southbound sur le plan de données.

1. **Communication northbound**. Contrôleur de réseau communique sur le plan de gestion avec un logiciel de gestion prenant en charge SDN\ tels que Windows PowerShell et \(SCVMM\) System Center Virtual Machine Manager. Ces outils de gestion vous fournissent la possibilité de définir la stratégie de réseau et de créer un état de l’objectif du réseau, par rapport à laquelle vous pouvez comparer la configuration réseau pour mettre à la configuration réelle à parité avec l’état de l’objectif.

2. **La Communication du Cluster contrôleur réseau**. Lorsque vous configurez trois ou plusieurs ordinateurs virtuels en tant que nœuds de cluster de contrôleur de réseau, ces nœuds communiquent entre eux. Cette communication peut être liée à la synchronisation et la réplication des données entre les nœuds ou la communication entre les services de contrôleur de réseau spécifique.

3.  **Communication southbound**. Contrôleur de réseau communique sur le plan de données avec l’infrastructure SDN et autres appareils tels que les équilibreurs de charge logicielle, les passerelles et les ordinateurs hôtes. Vous pouvez utiliser le contrôleur de réseau pour configurer et gérer ces périphériques southbound afin qu’ils conservent l’état de l’objectif que vous avez configurées pour le réseau.

## <a name="northbound-communication"></a>Communication northbound

Contrôleur de réseau prend en charge l’authentification, l’autorisation et le chiffrement pour la communication Northbound. Les sections suivantes fournissent des informations sur la façon de configurer ces paramètres de sécurité.

**Authentification**

Lorsque vous configurez l’authentification pour la communication Northbound du contrôleur réseau, vous autorisez les nœuds de cluster de contrôleur de réseau et les clients de gestion pour vérifier l’identité de l’appareil avec lequel ils communiquent.

Contrôleur de réseau prend en charge trois modes d’authentification entre les clients de gestion et les nœuds de contrôleur de réseau suivants.

>[!Note]
>Si vous déployez uniquement avec System Center Virtual Machine Manager, le contrôleur de réseau **Kerberos** mode est pris en charge.

1. **Kerberos**. Vous pouvez utiliser l’authentification Kerberos lorsque les deux le client de gestion, telles que l’ordinateur qui exécute SCVMM et tous les nœuds de cluster du contrôleur de réseau sont joints à un domaine Active Directory, avec des comptes de domaine utilisés pour l’authentification.

2. **X509**. X509 est l’authentification basée sur les certificate\. Vous pouvez utiliser X509 authentification lorsque les clients de gestion ne sont pas joints à un domaine Active Directory. Pour utiliser X509, vous devez inscrire des certificats sur tous les nœuds de cluster de contrôleur de réseau et les clients de gestion, et tous les nœuds et les clients de gestion doivent approuver les uns des autres certificats.

3. **Aucun**. Lorsque vous sélectionnez ce mode, aucune authentification n’est effectuée entre les clients de gestion et le contrôleur de réseau. Ce mode est fourni uniquement à des fins de test et n’est pas recommandé d’utiliser dans un environnement de production. 

Vous pouvez configurer le mode d’authentification pour la communication Northbound à l’aide de la commande Windows PowerShell **Install-NetworkController** avec la **ClientAuthentication** paramètre. 

Pour plus d’informations, consultez les rubriques suivantes. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Autorisation**

Lorsque vous configurez l’autorisation pour la communication Northbound du contrôleur réseau, vous autorisez les nœuds de cluster de contrôleur de réseau et les clients de gestion pour vérifier que l’appareil avec lequel ils communiquent est approuvé et est autorisé à participer à la communication.

Pour chacun des modes d’authentification pris en charge par le contrôleur de réseau, les méthodes d’autorisation suivants sont utilisés.

1.  **Kerberos**. Lorsque vous utilisez la méthode d’authentification Kerberos, vous définissez les utilisateurs et les ordinateurs qui sont autorisés à communiquer avec le contrôleur de réseau en créant un groupe de sécurité dans Active Directory et puis ajouter les utilisateurs autorisés et les ordinateurs au groupe. Vous pouvez configurer le contrôleur de réseau pour utiliser le groupe de sécurité pour l’autorisation à l’aide de la **ClientSecurityGroup** paramètre de la **Install-NetworkController** commande Windows PowerShell. Une fois que le contrôleur de réseau est installé, vous pouvez modifier le groupe de sécurité à l’aide de la [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller) commande avec le paramètre **- ClientSecurityGroup**. Si vous utilisez SCVMM, vous devez fournir le groupe de sécurité en tant que paramètre pendant le déploiement.

2.  **X509**. Lorsque vous utilisez le X509 méthode d’authentification, le contrôleur de réseau uniquement accepte les demandes des clients de gestion dont empreintes numériques de certificat sont connus de contrôleur de réseau. Vous pouvez configurer ces empreintes numériques à l’aide de la **ClientCertificateThumbprint** paramètre de la **Install-NetworkController** commande Windows PowerShell. Vous pouvez ajouter d’autres empreintes numériques de client à tout moment à l’aide de la **Set-NetworkController** commande.

3.  **Aucun**. Lorsque vous sélectionnez ce mode, il n’existe aucune autorisation effectuée pour les tentatives de communication entre les nœuds de contrôleur de réseau et les clients de gestion. Ce mode est fourni uniquement à des fins de test et n’est pas recommandé d’utiliser dans un environnement de production. 

Pour plus d’informations, consultez les rubriques suivantes. 

- [Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)
- [Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)

**Chiffrement**

Communication northbound utilise \(SSL\) (Secure Sockets Layer) pour créer un canal chiffré entre les nœuds de contrôleur de réseau et les clients de gestion. Le chiffrement SSL pour la communication Northbound inclut les conditions suivantes.

- Tous les nœuds de contrôleur de réseau doivent avoir un certificat qui inclut le cadre de l’authentification du serveur et l’authentification du Client dans les extensions d’utilisation améliorée de la clé \(EKU\) identique. 

- L’URI utilisé par les clients de gestion pour communiquer avec le contrôleur de réseau doit être le nom de sujet du certificat. Le nom de sujet du certificat doit contenir le nom de domaine complet (FQDN) ou l’adresse IP du point de terminaison REST contrôleur de réseau.

- Si les nœuds de contrôleur de réseau sont situés sur différents sous-réseaux, le nom d’objet de leurs certificats doit être identique à la valeur que vous utilisez pour le **RestName** paramètre dans le **Install-NetworkController** commande Windows PowerShell. 

- Le certificat SSL doit être approuvé par tous les clients de gestion.

### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuration et l’inscription de certificats SSL

Vous devez inscrire manuellement le certificat SSL sur les nœuds de contrôleur de réseau.

Une fois que le certificat est inscrit, vous pouvez configurer le contrôleur de réseau pour utiliser le certificat avec la **- certificats** paramètre de la **Install-NetworkController** commande Windows PowerShell. Si vous avez déjà installé le contrôleur de réseau, vous pouvez mettre à jour la configuration à tout moment à l’aide de la **Set-NetworkController** commande.

>[!NOTE]
>Si vous utilisez SCVMM, vous devez ajouter le certificat en tant que ressource de bibliothèque. Pour plus d’informations, voir [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Communications de Cluster de contrôleur de réseau

Contrôleur de réseau prend en charge l’authentification, l’autorisation et le chiffrement pour la communication entre les nœuds de contrôleur de réseau. La communication est effectuée via [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) et TCP.

Vous pouvez configurer ce mode avec la **ClusterAuthentication** paramètre de la **Install-NetworkControllerCluster** commande Windows PowerShell. 

Pour plus d’informations, voir [Install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

**Authentification**

Lorsque vous configurez l’authentification pour les communications de Cluster de contrôleur de réseau, vous autorisez les nœuds de cluster de contrôleur de réseau vérifier l’identité des autres nœuds avec lequel ils communiquent.

Contrôleur de réseau prend en charge trois modes d’authentification entre les nœuds de contrôleur de réseau suivants.

>[!NOTE]
>Si vous déployez un contrôleur de réseau à l’aide de SCVMM, uniquement **Kerberos** mode est pris en charge.

1. **Kerberos**. Vous pouvez utiliser l’authentification Kerberos lorsque tous les nœuds de cluster du contrôleur de réseau sont joints à un domaine Active Directory, avec des comptes de domaine utilisés pour l’authentification.

2. **X509**. X509 est l’authentification basée sur les certificate\. Vous pouvez utiliser X509 authentification lorsque le contrôleur de réseau des nœuds de cluster ne sont pas joints à un domaine Active Directory. Pour utiliser X509, vous devez inscrire des certificats à tous les nœuds de cluster du contrôleur de réseau, et tous les nœuds doivent approuver les certificats. En outre, le nom du sujet du certificat qui est inscrit sur chaque nœud doit être le même que le nom DNS du nœud.

3. **Aucun**. Lorsque vous sélectionnez ce mode, aucune authentification n’est effectuée entre les nœuds de contrôleur de réseau. Ce mode est fourni uniquement à des fins de test et n’est pas recommandé d’utiliser dans un environnement de production.

**Autorisation**

Lorsque vous configurez l’autorisation pour la communication du Cluster de contrôleur de réseau, vous autorisez les nœuds de cluster de contrôleur de réseau vérifier que les nœuds avec lequel ils communiquent sont approuvés et sont autorisés à participer à la communication.

Pour chacun des modes d’authentification pris en charge par le contrôleur de réseau, les méthodes d’autorisation suivants sont utilisés.

1. **Kerberos**. Les nœuds du contrôleur réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide de la **nom** paramètre de la [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) commande Windows PowerShell.

2. **X509**. Les nœuds du contrôleur réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide de la **nom** paramètre de la [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) commande Windows PowerShell.

3. **Aucun**. Lorsque vous sélectionnez ce mode, il n’existe aucune autorisation effectuée entre les nœuds de contrôleur de réseau. Ce mode est fourni uniquement à des fins de test et n’est pas recommandé d’utiliser dans un environnement de production.

**Chiffrement**

Communication entre les nœuds de contrôleur de réseau est chiffrée à l’aide de chiffrement au niveau du Transport WCF. Cette forme de chiffrement est utilisée lorsque les méthodes d’authentification et d’autorisation sont Kerberos ou X509 certificats. Pour plus d’informations, consultez les rubriques suivantes.

- [Comment: sécuriser un Service avec les informations d’identification Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Comment: sécuriser un Service avec des certificats X.509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Communication southbound

Contrôleur de réseau interagit avec différents types d’appareils pour la communication Southbound. Ces interactions utilisent différents protocoles. Pour cette raison, il existe des exigences différentes pour l’authentification, d’autorisation et le chiffrement en fonction du type d’appareil et le protocole utilisé par le contrôleur de réseau pour communiquer avec le périphérique.

Le tableau suivant fournit des informations sur l’interaction de contrôleur de réseau avec différents appareils southbound.

| Service/périphérique southbound | Protocole              | Authentification utilisé    |
|---------------------------|-----------------------|------------------------|
| Équilibreur de charge logicielle    | WCF (MUX), TCP (hôte) | Certificats           |
| Pare-feu                  | OVSDB                 | Certificats           |
| Passerelle                   | WinRM                 | Kerberos, les certificats |
| Réseau virtuel        | OVSDB, WCF            | Certificats           |
| Routage défini par l’utilisateur      | OVSDB                 | Certificats           |

Pour chacun de ces protocoles, le mécanisme de communication est décrite dans la section suivante.

**Authentification**

Pour la communication Southbound, les méthodes d’authentification et les protocoles suivants sont utilisés.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, l’authentification est effectuée à l’aide de X509 certificats. Contrôleur de réseau et de l’homologue \(SLB\) l’équilibrage de charge logicielle multiplexeur \ (MUX\) / ordinateurs hôtes présentent leurs certificats entre eux pour l’authentification mutuelle. Chaque certificat doit être approuvé par l’homologue distant.

    Pour l’authentification southbound, vous pouvez utiliser le même certificat SSL est configuré pour le chiffrement de la communication avec les clients Northbound. Vous devez également configurer un certificat sur les SLB MUX et les périphériques de l’ordinateur hôte. Le nom de sujet du certificat doit être identique au nom DNS de l’appareil.

2. **WinRM**. Pour ce protocole, l’authentification est effectuée à l’aide de Kerberos \ (pour virtuelles\ joint au domaine) et à l’aide de certificats \ (pour virtuelles\ joint à un domaine).

**Autorisation**

Pour la communication Southbound, les protocoles suivants et les méthodes d’autorisation sont utilisés.

1. **WCF/TCP**. Pour ces protocoles, autorisation est basée sur le nom d’objet de l’entité d’homologue. Contrôleur de réseau stocke le nom DNS de l’appareil homologue et l’utilise pour l’autorisation. Ce nom DNS doit correspondre au nom de sujet de l’appareil dans le certificat. De même, les certificats de contrôleur de réseau doivent correspondre le nom DNS du contrôleur réseau stocké sur l’appareil homologue.

2. **WinRM**. Si Kerberos est utilisé, le compte du client WinRM doit être présent dans un groupe prédéfini dans Active Directory ou dans le groupe Administrateurs Local sur le serveur. Si les certificats sont utilisés, le client présente un certificat pour le serveur que le serveur autorise à l’aide du nom d’objet/émetteur, et que le serveur utilise un compte d’utilisateur mappé pour effectuer l’authentification.

3.  **OVSDB**. Il n’existe aucune autorisation fournie pour ce protocole.

**Chiffrement**

Pour la communication Southbound, les méthodes de chiffrement suivants sont utilisés pour les protocoles.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, le chiffrement est effectué à l’aide du certificat qui est inscrit sur le serveur ou client.

2. **WinRM**. Trafic de WinRM est chiffré par défaut à l’aide du fournisseur SSP Kerberos \(SSP\). Vous pouvez configurer le cryptage supplémentaire, sous la forme de SSL, sur le serveur de WinRM.

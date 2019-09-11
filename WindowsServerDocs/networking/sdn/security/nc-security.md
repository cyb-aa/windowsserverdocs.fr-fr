---
title: Sécurité du contrôleur de réseau
description: Vous pouvez utiliser cette rubrique pour apprendre à configurer la sécurité de toutes les communications entre le contrôleur de réseau et d’autres logiciels et périphériques.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: cea660eb28645fb814d718ac04d0c9acea7b2e34
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867136"
---
# <a name="secure-the-network-controller"></a>Sécuriser le contrôleur de réseau

Dans cette rubrique, vous allez apprendre à configurer la sécurité de toutes les communications entre le [contrôleur de réseau](../technologies/network-controller/network-controller.md) et d’autres logiciels et périphériques. 

Les chemins de communication que vous pouvez sécuriser incluent la communication Northbound sur le plan de gestion, la communication entre \(les\) machines virtuelles du contrôleur de réseau et les machines virtuelles d’un cluster, et la communication Southbound sur les données verticale.

1. **Communication Northbound**. Le contrôleur de réseau communique sur le plan de\-gestion à l’aide d’un logiciel de \(gestion\)SDN tel que Windows PowerShell et System Center Virtual Machine Manager SCVMM. Ces outils de gestion vous permettent de définir une stratégie réseau et de créer un état d’objectif pour le réseau, par rapport auquel vous pouvez comparer la configuration de réseau réelle pour faire en sorte que la configuration réelle soit en parité avec l’état de l’objectif.

2. **Communication du cluster du contrôleur de réseau**. Quand vous configurez trois machines virtuelles ou plus en tant que nœuds de cluster de contrôleur de réseau, ces nœuds communiquent entre eux. Cette communication peut être liée à la synchronisation et à la réplication des données entre les nœuds, ou à une communication spécifique entre les services de contrôleur de réseau.

3.  **Communication Southbound**. Le contrôleur de réseau communique sur le plan de données avec l’infrastructure SDN et d’autres périphériques, tels que les équilibreurs de charge logicielle, les passerelles et les ordinateurs hôtes. Vous pouvez utiliser le contrôleur de réseau pour configurer et gérer ces appareils Southbound afin qu’ils maintiennent l’état d’objectif que vous avez configuré pour le réseau.


## <a name="northbound-communication"></a>Communication Northbound

Le contrôleur de réseau prend en charge l’authentification, l’autorisation et le chiffrement pour la communication Northbound. Les sections suivantes fournissent des informations sur la configuration de ces paramètres de sécurité.

### <a name="authentication"></a>Authentication

Quand vous configurez l’authentification pour la communication Northbound du contrôleur de réseau, vous autorisez les nœuds de cluster et les clients de gestion du contrôleur de réseau à vérifier l’identité de l’appareil avec lequel ils communiquent.

Le contrôleur de réseau prend en charge les trois modes d’authentification suivants entre les clients de gestion et les nœuds de contrôleur de réseau.

>[!Note]
>Si vous déployez un contrôleur de réseau avec System Center Virtual Machine Manager, seul le mode **Kerberos** est pris en charge.

1. **Kerberos**. Utilisez l’authentification Kerberos lors de la jointure du client de gestion et de tous les nœuds de cluster de contrôleur de réseau à un domaine Active Directory. Le domaine Active Directory doit avoir des comptes de domaine utilisés pour l’authentification.

2. **X509**. Utilisez x509 pour l'\-authentification basée sur les certificats pour les clients de gestion qui ne sont pas joints à un domaine Active Directory. Vous devez inscrire des certificats à tous les nœuds de cluster de contrôleur de réseau et aux clients de gestion. En outre, tous les nœuds et les clients de gestion doivent approuver les certificats des autres.

3. **Aucun**. Utilisez None à des fins de test dans un environnement de test et, par conséquent, n’est pas recommandé pour une utilisation dans un environnement de production. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds et les clients de gestion.

Vous pouvez configurer le mode d’authentification pour la communication Northbound à l’aide de la commande Windows PowerShell **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** avec le paramètre _clientauthentication_ . 


### <a name="authorization"></a>Authorization

Quand vous configurez l’autorisation pour la communication Northbound du contrôleur de réseau, vous autorisez les nœuds de cluster et les clients de gestion du contrôleur de réseau à vérifier que l’appareil avec lequel ils communiquent est approuvé et qu’il est autorisé à participer au communication.

Utilisez les méthodes d’autorisation suivantes pour chacun des modes d’authentification pris en charge par le contrôleur de réseau.

1.  **Kerberos**. Lorsque vous utilisez la méthode d’authentification Kerberos, vous définissez les utilisateurs et les ordinateurs autorisés à communiquer avec le contrôleur de réseau en créant un groupe de sécurité dans Active Directory, puis en ajoutant les utilisateurs et les ordinateurs autorisés au groupe. Vous pouvez configurer le contrôleur de réseau pour utiliser le groupe de sécurité pour l’autorisation à l’aide du paramètre _ClientSecurityGroup_ de la commande Windows PowerShell **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** . Après avoir installé le contrôleur de réseau, vous pouvez modifier le groupe de sécurité à l’aide de la commande **[Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** avec le paramètre _-ClientSecurityGroup_. Si vous utilisez SCVMM, vous devez fournir le groupe de sécurité en tant que paramètre au cours du déploiement.

2.  **X509**. Lorsque vous utilisez la méthode d’authentification x509, le contrôleur de réseau accepte uniquement les demandes des clients de gestion dont les empreintes numériques de certificat sont connues du contrôleur de réseau. Vous pouvez configurer ces empreintes numériques à l’aide du paramètre _ClientCertificateThumbprint_ de la commande Windows PowerShell **[install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** . Vous pouvez ajouter d’autres empreintes client à tout moment à l’aide de la commande **[Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** .

3.  **Aucun**. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds et les clients de gestion. Utilisez None à des fins de test dans un environnement de test et, par conséquent, n’est pas recommandé pour une utilisation dans un environnement de production. 


### <a name="encryption"></a>Chiffrement

La communication Northbound utilise \(protocole SSL\) SSL pour créer un canal chiffré entre les clients de gestion et les nœuds de contrôleur de réseau. Le chiffrement SSL pour la communication Northbound présente les exigences suivantes :

- Tous les nœuds de contrôleur de réseau doivent avoir un certificat identique qui comprend l’authentification du serveur et des rôles \(d'\) authentification du client dans les extensions EKU de l’utilisation améliorée de la clé. 

- L’URI utilisé par les clients de gestion pour communiquer avec le contrôleur de réseau doit être le nom du sujet du certificat. Le nom d’objet du certificat doit contenir le nom de domaine complet (FQDN) ou l’adresse IP du point de terminaison REST du contrôleur de réseau.

- Si les nœuds de contrôleur de réseau se trouvent sur des sous-réseaux différents, le nom d’objet de leurs certificats doit être identique à la valeur utilisée pour le paramètre _restname que_ dans la commande Windows PowerShell **install-NetworkController** . 

- Tous les clients de gestion doivent approuver le certificat SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Inscription et configuration du certificat SSL

Vous devez inscrire manuellement le certificat SSL sur les nœuds du contrôleur de réseau.

Une fois le certificat inscrit, vous pouvez configurer le contrôleur de réseau de façon à ce qu’il utilise le certificat avec le paramètre **-serverCertificate** de la commande Windows PowerShell **install-NetworkController** . Si vous avez déjà installé le contrôleur de réseau, vous pouvez mettre à jour la configuration à tout moment à l’aide de la commande **Set-NetworkController** .

>[!NOTE]
>Si vous utilisez SCVMM, vous devez ajouter le certificat en tant que ressource de bibliothèque. Pour plus d’informations, consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Communication du cluster du contrôleur de réseau

Le contrôleur de réseau prend en charge l’authentification, l’autorisation et le chiffrement pour la communication entre les nœuds du contrôleur de réseau. La communication est sur [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) et TCP.

Vous pouvez configurer ce mode avec le paramètre **ClusterAuthentication** de la commande Windows PowerShell **install-NetworkControllerCluster** . 

Pour plus d’informations, consultez [install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Authentication

Quand vous configurez l’authentification pour la communication du cluster du contrôleur de réseau, vous autorisez les nœuds du cluster de contrôleur de réseau à vérifier l’identité des autres nœuds avec lesquels ils communiquent.

Le contrôleur de réseau prend en charge les trois modes d’authentification suivants entre les nœuds de contrôleur de réseau.

>[!NOTE]
>Si vous déployez un contrôleur de réseau à l’aide de SCVMM, seul le mode **Kerberos** est pris en charge.

1. **Kerberos**. Vous pouvez utiliser l’authentification Kerberos lorsque tous les nœuds de cluster de contrôleur de réseau sont joints à un domaine Active Directory, avec les comptes de domaine utilisés pour l’authentification.

2. **X509**. X509 est l'\-authentification basée sur les certificats. Vous pouvez utiliser l’authentification x509 lorsque les nœuds de cluster du contrôleur de réseau ne sont pas joints à un domaine Active Directory. Pour utiliser x509, vous devez inscrire des certificats à tous les nœuds de cluster de contrôleur de réseau, et tous les nœuds doivent approuver les certificats. En outre, le nom du sujet du certificat qui est inscrit sur chaque nœud doit être le même que le nom DNS du nœud.

3. **Aucun**. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds du contrôleur de réseau. Ce mode n’est fourni qu’à des fins de test et n’est pas recommandé pour une utilisation dans un environnement de production.

### <a name="authorization"></a>Authorization

Quand vous configurez l’autorisation pour la communication du cluster du contrôleur de réseau, vous autorisez les nœuds du cluster de contrôleur de réseau à vérifier que les nœuds avec lesquels ils communiquent sont approuvés et qu’ils ont l’autorisation de participer à la communication.

Pour chacun des modes d’authentification pris en charge par le contrôleur de réseau, les méthodes d’autorisation suivantes sont utilisées.

1. **Kerberos**. Les nœuds de contrôleur de réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide du paramètre **Name** de la commande Windows PowerShell [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) .

2. **X509**. Les nœuds de contrôleur de réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide du paramètre **Name** de la commande Windows PowerShell [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) .

3. **Aucun**. Lorsque vous choisissez ce mode, aucune autorisation n’est effectuée entre les nœuds du contrôleur de réseau. Ce mode n’est fourni qu’à des fins de test et n’est pas recommandé pour une utilisation dans un environnement de production.

### <a name="encryption"></a>Chiffrement

La communication entre les nœuds du contrôleur de réseau est chiffrée à l’aide du chiffrement au niveau du transport WCF. Cette forme de chiffrement est utilisée lorsque les méthodes d’authentification et d’autorisation sont des certificats Kerberos ou x509. Pour plus d’informations, voir les rubriques suivantes :

- [Guide pratique pour Sécuriser un service avec les informations d’identification Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Guide pratique pour Sécuriser un service avec des certificats](https://msdn.microsoft.com/library/ms788968.aspx)X. 509.

## <a name="southbound-communication"></a>Communication Southbound

Le contrôleur de réseau interagit avec différents types d’appareils pour la communication Southbound. Ces interactions utilisent des protocoles différents. Pour cette raison, il existe des exigences différentes pour l’authentification, l’autorisation et le chiffrement selon le type d’appareil et le protocole utilisés par le contrôleur de réseau pour communiquer avec l’appareil.

Le tableau suivant fournit des informations sur l’interaction du contrôleur de réseau avec différents appareils Southbound.

| Appareil/service Southbound | Protocol              | Authentification utilisée    |
|---------------------------|-----------------------|------------------------|
| Équilibrage de la charge logicielle    | WCF (MUX), TCP (hôte) | Certificats           |
| Pare-feu                  | OVSDB                 | Certificats           |
| Passerelle                   | WinRM                 | Kerberos, certificats |
| Réseau virtuel        | OVSDB, WCF            | Certificats           |
| Routage défini par l’utilisateur      | OVSDB                 | Certificats           |

Pour chacun de ces protocoles, le mécanisme de communication est décrit dans la section suivante.

### <a name="authentication"></a>Authentication

Pour la communication Southbound, les protocoles et méthodes d’authentification suivants sont utilisés.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, l’authentification s’effectue à l’aide de certificats X509. Les ordinateurs du contrôleur de réseau et de l' \(équilibrage\) de charge \(du\)logiciel homologue multiplexeur MUX/Hostent les certificats entre eux pour l’authentification mutuelle. Chaque certificat doit être approuvé par l’homologue distant.

    Pour l’authentification Southbound, vous pouvez utiliser le même certificat SSL que celui configuré pour chiffrer la communication avec les clients Northbound. Vous devez également configurer un certificat sur les appareils hôtes et MUX SLB. Le nom d’objet du certificat doit être le même que le nom DNS de l’appareil.

2. **WinRM**. Pour ce protocole, l’authentification est effectuée à l' \(aide de Kerberos pour\) les ordinateurs joints à \(un domaine et à l’aide\)de certificats pour les ordinateurs non joints à un domaine.

### <a name="authorization"></a>Authorization

Pour la communication Southbound, les protocoles et les méthodes d’autorisation suivants sont utilisés.

1. **WCF/TCP**. Pour ces protocoles, l’autorisation est basée sur le nom d’objet de l’entité homologue. Le contrôleur de réseau stocke le nom DNS du périphérique homologue et l’utilise pour l’autorisation. Ce nom DNS doit correspondre au nom d’objet de l’appareil dans le certificat. De même, le certificat de contrôleur de réseau doit correspondre au nom DNS du contrôleur de réseau stocké sur l’appareil homologue.

2. **WinRM**. Si Kerberos est utilisé, le compte client WinRM doit être présent dans un groupe prédéfini dans Active Directory ou dans le groupe Administrateurs local sur le serveur. Si des certificats sont utilisés, le client présente un certificat au serveur que le serveur autorise à l’aide du nom d’objet ou de l’émetteur, et le serveur utilise un compte d’utilisateur mappé pour effectuer l’authentification.

3.  **OVSDB**. Aucune autorisation n’est fournie pour ce protocole.

### <a name="encryption"></a>Chiffrement

Pour la communication Southbound, les méthodes de chiffrement suivantes sont utilisées pour les protocoles.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, le chiffrement est effectué à l’aide du certificat qui est inscrit sur le client ou le serveur.

2. **WinRM**. Le trafic WinRM est chiffré par défaut à l’aide du fournisseur \(SSP\)(Security Support Provider) Kerberos. Vous pouvez configurer un chiffrement supplémentaire, sous la forme de SSL, sur le serveur WinRM.

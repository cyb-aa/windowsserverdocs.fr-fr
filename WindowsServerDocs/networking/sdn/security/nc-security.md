---
title: Sécurité du contrôleur de réseau
description: Vous pouvez utiliser cette rubrique pour savoir comment configurer la sécurité pour toutes les communications entre le contrôleur de réseau et les autres logiciels et périphériques.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: bc625de9-ee31-40a4-9ad2-7448bfbfb6e6
ms.author: pashort
author: shortpatti
ms.date: 08/30/2018
ms.openlocfilehash: fccd6ee1ba734d72629264bd5b3bb7d93ddcdb70
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884760"
---
# <a name="secure-the-network-controller"></a>Sécuriser le contrôleur de réseau

Dans cette rubrique, vous allez apprendre à configurer la sécurité pour toutes les communications entre [contrôleur de réseau](../technologies/network-controller/network-controller.md) et les autres logiciels et périphériques. 

Les chemins de communication que vous pouvez sécuriser incluent une communication Northbound du plan de gestion, les communications de cluster entre les machines virtuelles du contrôleur de réseau \(machines virtuelles\) dans un cluster et la communication Southbound sur les données plan.

1. **Communication northbound**. Contrôleur de réseau communique sur le plan de gestion avec SDN\-logiciel capable de gestion telles que Windows PowerShell et System Center Virtual Machine Manager \(SCVMM\). Ces outils de gestion vous permet de fournir la possibilité de définir la stratégie de réseau et pour créer un état d’objectif pour le réseau, par rapport à laquelle vous pouvez comparer la configuration de réseau réelle pour rendre la configuration réelle parité avec l’état de l’objectif.

2. **La Communication de Cluster de contrôleur réseau**. Lorsque vous configurez au moins trois machines virtuelles en tant que nœuds de cluster de contrôleur de réseau, ces nœuds communiquent entre eux. Cette communication peut être liée à la synchronisation et la réplication des données entre les nœuds ou de communication spécifiques entre les services de contrôleur de réseau.

3.  **Communication southbound**. Contrôleur de réseau communique sur le plan de données avec l’infrastructure SDN et autres dispositifs tels que les équilibreurs de charge logiciels, les passerelles et les ordinateurs hôtes. Vous pouvez utiliser le contrôleur de réseau pour configurer et gérer ces appareils southbound afin qu’elles conservent l’état de l’objectif que vous avez configuré pour le réseau.


## <a name="northbound-communication"></a>Communication northbound

Contrôleur de réseau prend en charge l’authentification, autorisation et le chiffrement pour la communication Northbound. Les sections suivantes fournissent des informations sur la façon de configurer ces paramètres de sécurité.

### <a name="authentication"></a>Authentification

Lorsque vous configurez l’authentification pour la communication Northbound du contrôleur réseau, vous autorisez les nœuds de cluster de contrôleur de réseau et de gestion aux clients de vérifier l’identité de l’appareil avec lequel ils communiquent.

Contrôleur de réseau prend en charge trois modes d’authentification entre les clients de gestion et de nœuds de contrôleur de réseau suivants.

>[!Note]
>Si vous déployez uniquement le contrôleur de réseau avec System Center Virtual Machine Manager, **Kerberos** mode est pris en charge.

1. **Kerberos**. Utiliser l’authentification Kerberos lors de la jointure à la fois le client de gestion et tous les nœuds de cluster de contrôleur de réseau à un domaine Active Directory. Le domaine Active Directory doit avoir des comptes de domaine utilisés pour l’authentification.

2. **X509**. Utilisez X509 pour certificat\-en fonction de l’authentification pour les clients de gestion ne pas joint à un domaine Active Directory. Vous devez inscrire des certificats sur tous les nœuds de cluster de contrôleur de réseau et les clients de gestion. En outre, tous les nœuds et les clients de gestion doivent approuver mutuellement leurs certificats.

3. **Aucun**. Utilisation None pour le test à des fins dans un environnement de test et, par conséquent, pas recommandé pour une utilisation dans un environnement de production. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds et les clients de gestion.

Vous pouvez configurer le mode d’authentification pour la communication Northbound à l’aide de la commande Windows PowerShell **[Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** avec la _ClientAuthentication_ paramètre. 


### <a name="authorization"></a>Authorization

Lorsque vous configurez l’autorisation pour la communication Northbound du contrôleur réseau, vous autorisez les nœuds de cluster de contrôleur de réseau et de gestion aux clients de vérifier que l’appareil avec lequel ils communiquent est approuvée et qu’il est autorisé à participer à la communication.

Utiliser les méthodes d’autorisation suivantes pour chacun des modes d’authentification pris en charge par le contrôleur de réseau.

1.  **Kerberos**. Lorsque vous utilisez la méthode d’authentification Kerberos, vous définissez les utilisateurs et les ordinateurs autorisés à communiquer avec le contrôleur de réseau en créant un groupe de sécurité dans Active Directory et puis en ajoutant les utilisateurs autorisés et les ordinateurs au groupe. Vous pouvez configurer le contrôleur de réseau pour utiliser le groupe de sécurité pour l’autorisation à l’aide de la _ClientSecurityGroup_ paramètre de la **[Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** Commande de Windows PowerShell. Après avoir installé le contrôleur de réseau, vous pouvez modifier le groupe de sécurité à l’aide de la **[Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** commande avec le paramètre _- ClientSecurityGroup_ . Si vous utilisez SCVMM, vous devez fournir le groupe de sécurité en tant que paramètre au cours du déploiement.

2.  **X509**. Lorsque vous utilisez la X509 méthode d’authentification, contrôleur de réseau accepte uniquement les demandes des clients de gestion dont empreintes de certificat sont connus au contrôleur de réseau. Vous pouvez configurer ces empreintes numériques à l’aide de la _ClientCertificateThumbprint_ paramètre de la **[Install-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontroller)** commande Windows PowerShell. Vous pouvez ajouter des empreintes numériques des autres clients à tout moment en utilisant le **[Set-NetworkController](https://technet.microsoft.com/itpro/powershell/windows/network-controller/set-networkcontroller)** commande.

3.  **Aucun**. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds et les clients de gestion. Utilisation None pour le test à des fins dans un environnement de test et, par conséquent, pas recommandé pour une utilisation dans un environnement de production. 


### <a name="encryption"></a>Chiffrement

Communication northbound utilise le protocole SSL (Secure Sockets Layer) \(SSL\) pour créer un canal chiffré entre les clients de gestion et de nœuds de contrôleur de réseau. Le chiffrement SSL pour la communication Northbound inclut les spécifications suivantes :

- Tous les nœuds de contrôleur de réseau doivent avoir un certificat identique qui inclut le cadre de l’authentification du serveur et l’authentification du Client dans Enhanced Key Usage \(EKU\) extensions. 

- L’URI utilisé par les clients de gestion pour communiquer avec le contrôleur de réseau doit être le nom de sujet du certificat. Le nom de sujet du certificat doit contenir le nom de domaine complet (FQDN) ou l’adresse IP du point de terminaison REST de contrôleur de réseau.

- Si les nœuds de contrôleur de réseau se trouvent sur des sous-réseaux différents, le nom de sujet de leurs certificats doit être identique à la valeur utilisée pour le _restname que_ paramètre dans le **Install-NetworkController** Windows Commande de PowerShell. 

- Tous les clients de gestion doivent approuver le certificat SSL.


### <a name="ssl-certificate-enrollment-and-configuration"></a>Configuration et inscription de certificats SSL

Vous devez inscrire manuellement le certificat SSL sur les nœuds de contrôleur de réseau.

Une fois le certificat est inscrit, vous pouvez configurer le contrôleur de réseau pour utiliser le certificat avec le **- ServerCertificate** paramètre de la **Install-NetworkController** commande Windows PowerShell . Si vous avez déjà installé le contrôleur de réseau, vous pouvez mettre à jour la configuration à tout moment à l’aide de la **Set-NetworkController** commande.

>[!NOTE]
>Si vous utilisez SCVMM, vous devez ajouter le certificat en tant que ressource de bibliothèque. Pour plus d’informations, consultez [configurer un contrôleur de réseau SDN dans l’infrastructure VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

## <a name="network-controller-cluster-communication"></a>Communication de Cluster de contrôleur de réseau

Contrôleur de réseau prend en charge l’authentification, autorisation et le chiffrement pour la communication entre les nœuds du contrôleur de réseau. La communication est sur [Windows Communication Foundation](https://msdn.microsoft.com/library/ms731082.aspx) \(WCF\) et TCP.

Vous pouvez configurer ce mode avec la **ClusterAuthentication** paramètre de la **Install-NetworkControllerCluster** commande Windows PowerShell. 

Pour plus d’informations, consultez [Install-NetworkControllerCluster](https://technet.microsoft.com/itpro/powershell/windows/network-controller/install-networkcontrollercluster).

### <a name="authentication"></a>Authentification

Lorsque vous configurez l’authentification pour la communication de Cluster de contrôleur de réseau, vous autorisez les nœuds de cluster de contrôleur de réseau vérifier l’identité des autres nœuds avec lequel ils communiquent.

Contrôleur de réseau prend en charge trois modes d’authentification entre les nœuds du contrôleur de réseau suivants.

>[!NOTE]
>Si vous déployez un contrôleur de réseau à l’aide de SCVMM, uniquement **Kerberos** mode est pris en charge.

1. **Kerberos**. Vous pouvez utiliser l’authentification Kerberos lorsque tous les nœuds de cluster de contrôleur de réseau sont joints à un domaine Active Directory, avec les comptes de domaine utilisés pour l’authentification.

2. **X509**. X509 est certificat\-en fonction de l’authentification. Vous pouvez utiliser X509 authentification lorsque les nœuds de cluster de contrôleur de réseau ne sont pas joints à un domaine Active Directory. Pour utiliser X509, vous devez inscrire des certificats sur tous les nœuds de cluster de contrôleur de réseau, et tous les nœuds doivent approuver les certificats. En outre, le nom du sujet du certificat qui est inscrit sur chaque nœud doit être le même que le nom DNS du nœud.

3. **Aucun**. Lorsque vous choisissez ce mode, aucune authentification n’est effectuée entre les nœuds du contrôleur de réseau. Ce mode est fourni exclusivement à des fins de test et n’est pas recommandé pour une utilisation dans un environnement de production.

### <a name="authorization"></a>Authorization

Lorsque vous configurez l’autorisation pour la communication de Cluster de contrôleur de réseau, vous autorisez les nœuds de cluster de contrôleur de réseau pour vérifier que les nœuds avec lesquels ils communiquent sont approuvés et êtes autorisé à participer à la communication.

Pour chacun des modes d’authentification pris en charge par le contrôleur de réseau, les méthodes d’autorisation suivantes sont utilisées.

1. **Kerberos**. Nœuds de contrôleur de réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide de la **nom** paramètre de la [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) commande Windows PowerShell.

2. **X509**. Nœuds de contrôleur de réseau acceptent les demandes de communication uniquement à partir d’autres comptes d’ordinateur de contrôleur de réseau. Vous pouvez configurer ces comptes lorsque vous déployez un contrôleur de réseau à l’aide de la **nom** paramètre de la [New-NetworkControllerNodeObject](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollernodeobject) commande Windows PowerShell.

3. **Aucun**. Lorsque vous choisissez ce mode, il n’existe aucune autorisation effectuée entre les nœuds du contrôleur de réseau. Ce mode est fourni exclusivement à des fins de test et n’est pas recommandé pour une utilisation dans un environnement de production.

### <a name="encryption"></a>Chiffrement

Communication entre les nœuds du contrôleur de réseau est chiffrée à l’aide du chiffrement au niveau du Transport WCF. Cette forme de chiffrement est utilisée lorsque les méthodes d’authentification et d’autorisation sont Kerberos ou X509 certificats. Pour plus d’informations, voir les rubriques suivantes :

- [Procédure : Sécuriser un Service avec les informations d’identification Windows](https://msdn.microsoft.com/library/ms734673.aspx)
- [Procédure : Sécuriser un Service avec les certificats X.509](https://msdn.microsoft.com/library/ms788968.aspx).

## <a name="southbound-communication"></a>Communication southbound

Contrôleur de réseau interagit avec différents types d’appareils pour la communication Southbound. Ces interactions utilisent différents protocoles. Pour cette raison, il existe des exigences différentes pour l’authentification, l’autorisation et chiffrement selon le type d’appareil et le protocole utilisé par le contrôleur de réseau pour communiquer avec l’appareil.

Le tableau suivant fournit des informations sur l’interaction de contrôleur de réseau avec différents appareils southbound.

| APPAREIL southbound/service | Protocole              | Authentification utilisé    |
|---------------------------|-----------------------|------------------------|
| Équilibrage de la charge logicielle    | WCF (MUX), TCP (hôte) | Certificats           |
| Pare-feu                  | OVSDB                 | Certificats           |
| Passerelle                   | WinRM                 | Kerberos, certificats |
| Réseau virtuel        | OVSDB, WCF            | Certificats           |
| Routage défini par l’utilisateur      | OVSDB                 | Certificats           |

Pour chacun de ces protocoles, le mécanisme de communication est décrite dans la section suivante.

### <a name="authentication"></a>Authentification

Pour la communication Southbound, les méthodes d’authentification et les protocoles suivants sont utilisés.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, l’authentification est effectuée à l’aide de X509 certificats. Contrôleur de réseau et l’homologue de l’équilibrage de charge logiciel \(SLB\) multiplexeur \(MUX \) /ordinateurs hôtes présentent leurs certificats entre eux pour l’authentification mutuelle. Chaque certificat doit être approuvé par l’homologue distant.

    Pour l’authentification southbound, vous pouvez utiliser le même certificat SSL est configuré pour le chiffrement de la communication avec les clients Northbound. Vous devez également configurer un certificat sur des appareils de l’hôte et SLB MUX. Le nom de sujet du certificat doit être identique au nom DNS de l’appareil.

2. **WinRM**. Pour ce protocole, l’authentification est effectuée à l’aide de Kerberos \(pour le domaine des ordinateurs joints à un\) et à l’aide de certificats \(pour n’appartenant pas au domaine des ordinateurs joints à un\).

### <a name="authorization"></a>Authorization

Pour la communication Southbound, les protocoles suivants et les méthodes d’autorisation sont utilisées.

1. **WCF/TCP**. Pour ces protocoles, l’autorisation est basée sur le nom de sujet de l’entité pair. Contrôleur de réseau enregistre le nom DNS du périphérique homologue et l’utilise pour l’autorisation. Ce nom DNS doit correspondre au nom de sujet de l’appareil dans le certificat. De même, les certificats de contrôleur de réseau doit correspondre au nom de DNS du contrôleur réseau stocké sur l’appareil homologue.

2. **WinRM**. Si Kerberos est utilisé, le compte du client WinRM doit être présent dans un groupe prédéfini dans Active Directory ou dans le groupe Administrateurs Local sur le serveur. Si les certificats sont utilisés, le client présente un certificat sur le serveur que le serveur autorise à l’aide de l’émetteur/nom du sujet, et que le serveur utilise un compte d’utilisateur mappé pour effectuer l’authentification.

3.  **OVSDB**. Il n’existe aucune autorisation fournie pour ce protocole.

### <a name="encryption"></a>Chiffrement

Pour la communication Southbound, les méthodes de chiffrement suivantes sont utilisées pour les protocoles.

1. **WCF/TCP/OVSDB**. Pour ces protocoles, le chiffrement est effectué en utilisant le certificat qui est inscrit sur le serveur ou client.

2. **WinRM**. Le trafic de WinRM est chiffré par défaut à l’aide du fournisseur SSP Kerberos \(SSP\). Vous pouvez configurer un chiffrement supplémentaire, sous la forme de SSL, sur le serveur de WinRM.

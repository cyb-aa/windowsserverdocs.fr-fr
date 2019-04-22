---
title: Déployer le contrôleur de réseau à l’aide de Windows PowerShell
description: Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur un ou plusieurs ordinateurs ou des machines virtuelles (VM) qui exécutent Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: pashort
author: shortpatti
ms.date: 08/23/2018
ms.openlocfilehash: 31c1579dc840f6f4eb805ac4e10f51192a6b4c99
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816190"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Déployer le contrôleur de réseau à l’aide de Windows PowerShell

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur un ou plusieurs machines virtuelles (VM qui exécutent Windows Server 2016).

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(machine virtuelle\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur des machines virtuelles sur trois Hyper différents\-hôtes V, vous devez activer l’Hyper\-hôtes V pour Sdn \(SDN\) en ajoutant les ordinateurs hôtes à l’aide de contrôleur de réseau la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous permettez à l’équilibreur de charge logiciel SDN de la fonction. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Cette rubrique contient les sections suivantes.

- [Installer le rôle de serveur de contrôleur de réseau](#bkmk_role)

- [Configurer le cluster de contrôleur de réseau](#bkmk_configure)

- [Configurer l’application de contrôleur de réseau](#bkmk_app)

- [Validation de déploiement de contrôleur de réseau](#bkmk_validation)

- [Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau](#bkmk_ps)

- [Exemple de script de configuration contrôleur de réseau](#bkmk_script)

- [Étapes de post-déploiement pour les déploiements de Non-Kerberos](#bkmk_nonkerb)

## <a name="install-the-network-controller-server-role"></a>Installer le rôle de serveur de contrôleur de réseau

Vous pouvez utiliser cette procédure pour installer le rôle de serveur de contrôleur de réseau sur une machine virtuelle \(machine virtuelle\).

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(machine virtuelle\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur des machines virtuelles sur trois Hyper différents\-hôtes V, vous devez activer l’Hyper\-hôtes V pour Sdn \(SDN\) en ajoutant les ordinateurs hôtes au contrôleur de réseau. En procédant ainsi, vous permettez à l’équilibreur de charge logiciel SDN de la fonction.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  

>[!NOTE]
>Si vous souhaitez utiliser le Gestionnaire de serveur au lieu de Windows PowerShell pour installer le contrôleur de réseau, consultez [installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](https://technet.microsoft.com/library/mt403348.aspx)

Pour installer le contrôleur de réseau à l’aide de Windows PowerShell, tapez les commandes suivantes à l’invite de Windows PowerShell, puis appuyez sur ENTRÉE.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Installation du contrôleur de réseau nécessite que vous redémarrez l’ordinateur. Pour ce faire, tapez la commande suivante, puis appuyez sur ENTRÉE.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurer le cluster de contrôleur de réseau

Le cluster de contrôleur de réseau fournit une haute disponibilité et l’évolutivité à l’application de contrôleur de réseau, vous pouvez configurer après la création du cluster, et qui est hébergé sur le cluster.

>[!NOTE]
>Vous pouvez effectuer les procédures décrites dans les sections suivantes soit directement sur la machine virtuelle où vous avez installé le contrôleur de réseau, ou vous pouvez utiliser à distance Server Administration Tools pour Windows Server 2016 pour effectuer les procédures à partir d’un ordinateur distant qui est en cours d’exécution Windows Server 2016 ou Windows 10. En outre, l’appartenance au **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure. Si l’ordinateur ou une machine virtuelle sur laquelle vous avez installé le contrôleur de réseau est joint à un domaine, votre compte d’utilisateur doit être un membre de **utilisateurs du domaine**.

Vous pouvez créer un cluster de contrôleur de réseau en créant un objet de nœud, puis en configurant le cluster.

### <a name="create-a-node-object"></a>Créer un objet de nœud

Vous devez créer un objet de nœud pour chaque machine virtuelle qui est membre du cluster de contrôleur de réseau.

Pour créer un objet de nœud, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs pour chaque paramètre qui sont appropriées pour votre déploiement.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **New-NetworkControllerNodeObject** commande.

|Paramètre|Description|
|-------------|---------------|
|Nom|Le **nom** paramètre spécifie le nom convivial du serveur que vous souhaitez ajouter au cluster|
|Server|Le **Server** paramètre spécifie le nom d’hôte, le nom de domaine complet (FQDN) ou l’adresse IP du serveur que vous souhaitez ajouter au cluster. Pour les ordinateurs joints au domaine, nom de domaine complet est requis.|
|FaultDomain|Le **FaultDomain** paramètre spécifie le domaine de défaillance pour le serveur que vous ajoutez au cluster. Ce paramètre définit les serveurs qui peuvent rencontrer un échec en même temps que le serveur que vous ajoutez au cluster. Cet échec peut être dû à des dépendances physiques partagées telles que la puissance et de sources de mise en réseau. Domaines d’erreur représentent généralement les hiérarchies associées à ces dépendances partagées, avec des serveurs plus susceptibles d’échouer ensemble depuis un point plus élevé dans l’arborescence de domaine d’erreur. Pendant l’exécution, le contrôleur de réseau considère les domaines d’erreur dans le cluster et tente de répartir les services de contrôleur de réseau afin qu’ils soient dans des domaines d’erreur. Ce processus permet de s’assurer, en cas d’échec de tout un domaine d’erreur, que la disponibilité de ce service et son état n’est pas compromise. Domaines d’erreur sont spécifiés dans un format hiérarchique. Exemple : « Fd : / DC1/Rack1/Host1 », où DC1 est le nom du centre de données, rack 1 est le nom de rack et Host1 est le nom de l’hôte où le nœud est placé.|
|RestInterface|Le **RestInterface** paramètre spécifie le nom de l’interface sur le nœud où la communication Representational State Transfer (REST) est arrêtée. Cette interface de contrôleur de réseau reçoit des demandes de l’API Northbound à partir de la couche de gestion du réseau.|
|NodeCertificate|Le **NodeCertificate** paramètre spécifie le certificat de contrôleur de réseau utilise pour l’authentification d’ordinateur. Le certificat est requis si vous utilisez l’authentification basée sur certificat pour la communication au sein du cluster ; le certificat est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Le nom de sujet du certificat doit être identique au nom DNS du nœud.|

### <a name="configure-the-cluster"></a>Configurer le cluster

Pour configurer le cluster, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs pour chaque paramètre qui sont appropriées pour votre déploiement.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **Install-NetworkControllerCluster** commande.
  
|Paramètre|Description|
|-------------|---------------|
|ClusterAuthentication|Le **ClusterAuthentication** paramètre spécifie le type d’authentification qui est utilisé pour sécuriser la communication entre les nœuds et est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **X509** et **aucun**. L’authentification Kerberos utilise des comptes de domaine et est utilisable uniquement si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur les X509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez configurer manuellement le certificat avant d’exécuter cette commande.|
|ManagementSecurityGroup|Le **ManagementSecurityGroup** paramètre spécifie le nom du groupe de sécurité contenant les utilisateurs qui sont autorisés à exécuter les applets de commande de gestion à partir d’un ordinateur distant. Cela s’applique uniquement si ClusterAuthentication est Kerberos. Vous devez spécifier un groupe de sécurité de domaine et non un groupe de sécurité sur l’ordinateur local.|
|Nœud|Le **nœud** paramètre spécifie la liste des nœuds de contrôleur de réseau que vous avez créé à l’aide de la **New-NetworkControllerNodeObject** commande.|
|DiagnosticLogLocation|Le **DiagnosticLogLocation** paramètre spécifie l’emplacement du partage où les journaux de diagnostic sont régulièrement chargés. Si vous ne spécifiez pas une valeur pour ce paramètre, les journaux sont stockés localement sur chaque nœud. Les journaux sont stockés localement dans le systemdrive%\Windows\tracing\SDNDiagnostics % dossier. Journaux de cluster sont stockées localement dans le dossier de %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|Le **LogLocationCredential** paramètre spécifie les informations d’identification qui sont nécessaires pour accéder à l’emplacement du partage dans lequel les journaux sont stockés.|
|CredentialEncryptionCertificate|Le **CredentialEncryptionCertificate** paramètre spécifie le certificat de contrôleur de réseau utilise pour chiffrer les informations d’identification permettant d’accéder aux fichiers binaires du contrôleur de réseau et le  **LogLocationCredential**, si spécifiée. Le certificat doit être configuré sur tous les nœuds de contrôleur de réseau avant d’exécuter cette commande, et le même certificat doit être inscrits sur tous les nœuds du cluster. À l’aide de ce paramètre pour protéger les journaux et les fichiers binaires du contrôleur de réseau est recommandée dans les environnements de production. Sans ce paramètre, les informations d’identification sont stockées en texte clair et peuvent être utilisés à mauvais escient par tout utilisateur non autorisé.|
|Credential|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **informations d’identification** paramètre spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **CertificateThumbprint** paramètre spécifie numérique certificat de clé publique (X509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **UseSSL** paramètre spécifie le protocole Secure Sockets Layer (SSL) qui est utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n'est pas utilisé.|
|ComputerName|Le **ComputerName** paramètre spécifie le nœud de contrôleur de réseau sur lequel cette commande est exécuté. Si vous ne spécifiez pas une valeur pour ce paramètre, l’ordinateur local est utilisé par défaut.|
|LogSizeLimitInMBs|Ce paramètre spécifie la taille maximale du journal, en Mo, contrôleur de réseau capable de stocker. Journaux sont stockés de manière circulaire. Si DiagnosticLogLocation est fournie, la valeur par défaut de ce paramètre est de 40 Go. Si DiagnosticLogLocation n’est pas fournie, les journaux sont stockés sur les nœuds de contrôleur de réseau et la valeur par défaut de ce paramètre est de 15 Go.|
|LogTimeLimitInDays|Ce paramètre spécifie la limite de durée, en jours, pour lequel les journaux sont stockés. Journaux sont stockés de manière circulaire. La valeur par défaut de ce paramètre est de 3 jours.|

## <a name="configure-the-network-controller-application"></a>Configurer l’application de contrôleur de réseau
Pour configurer l’application de contrôleur de réseau, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs pour chaque paramètre qui sont appropriées pour votre déploiement.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **Install-NetworkController** commande.

|Paramètre|Description|
|-------------|---------------|
|ClientAuthentication|Le **ClientAuthentication** paramètre spécifie le type d’authentification qui est utilisé pour sécuriser la communication entre le REST et le contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **X509** et **aucun**. L’authentification Kerberos utilise des comptes de domaine et est utilisable uniquement si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur les X509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez configurer manuellement le certificat avant d’exécuter cette commande.|
|Nœud|Le **nœud** paramètre spécifie la liste des nœuds de contrôleur de réseau que vous avez créé à l’aide de la **New-NetworkControllerNodeObject** commande.|
|ClientCertificateThumbprint|Ce paramètre est obligatoire uniquement lorsque vous utilisez l’authentification basée sur certificat pour les clients du contrôleur de réseau. Le **ClientCertificateThumbprint** paramètre spécifie l’empreinte numérique du certificat inscrit auprès des clients sur la couche Northbound.|
|ServerCertificate|Le **ServerCertificate** paramètre spécifie le certificat de contrôleur de réseau utilise pour prouver son identité aux clients. Le certificat de serveur doit inclure l’objectif de l’authentification du serveur dans les extensions d’utilisation améliorée de la clé et doit être émis par une autorité de certification approuvée par les clients au contrôleur de réseau.|
|RESTIPAddress|Vous n’avez pas besoin de spécifier une valeur pour **RESTIPAddress** avec un déploiement à nœud unique du contrôleur de réseau. Pour les déploiements de plusieurs nœuds, le **RESTIPAddress** paramètre spécifie l’adresse IP du point de terminaison REST en notation CIDR. Par exemple, 192.168.1.10/24. La valeur de nom de l’objet de **ServerCertificate** doit correspondre à la valeur de la **RESTIPAddress** paramètre. Ce paramètre doit être spécifié pour tous les déploiements de contrôleur de réseau de plusieurs nœuds lorsque tous les nœuds se trouvent sur le même sous-réseau. Si les nœuds se trouvent sur des sous-réseaux différents, vous devez utiliser le **restname que** paramètre au lieu d’utiliser **RESTIPAddress**.|
|RestName|Vous n’avez pas besoin de spécifier une valeur pour **restname que** avec un déploiement à nœud unique du contrôleur de réseau. Le seul moment où vous devez spécifier une valeur pour **restname que** est lors de déploiements de plusieurs nœuds ont des nœuds qui se trouvent sur des sous-réseaux différents. Pour les déploiements de plusieurs nœuds, le **restname que** paramètre spécifie le nom de domaine complet pour le cluster de contrôleur de réseau.|
|ClientSecurityGroup|Le **ClientSecurityGroup** paramètre spécifie le nom du groupe de sécurité Active Directory dont les membres sont les clients du contrôleur de réseau. Ce paramètre est obligatoire uniquement si vous utilisez l’authentification Kerberos pour **ClientAuthentication**. Le groupe de sécurité doit contenir les comptes à partir duquel les API REST sont accessibles, et vous devez créer le groupe de sécurité et ajouter des membres avant d’exécuter cette commande.|
|Credential|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **informations d’identification** paramètre spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **CertificateThumbprint** paramètre spécifie numérique certificat de clé publique (X509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **UseSSL** paramètre spécifie le protocole Secure Sockets Layer (SSL) qui est utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n'est pas utilisé.|

Après avoir terminé la configuration de l’application de contrôleur de réseau, votre déploiement de contrôleur de réseau est terminée.

## <a name="network-controller-deployment-validation"></a>Validation de déploiement de contrôleur de réseau

Pour valider votre déploiement de contrôleur de réseau, vous pouvez ajouter des informations d’identification pour le contrôleur de réseau et ensuite récupérer les informations d’identification.

Si vous utilisez Kerberos comme mécanisme de ClientAuthentication, l’appartenance à la **ClientSecurityGroup** que vous avez créée est la condition minimale requise pour effectuer cette procédure.

**Procédure :**

1.  Sur un ordinateur client, si vous utilisez Kerberos comme mécanisme de ClientAuthentication, ouvrez une session avec un compte d’utilisateur qui est un membre de votre **ClientSecurityGroup**.

2. Ouvrez Windows PowerShell, tapez les commandes suivantes pour ajouter des informations d’identification au contrôleur de réseau, puis appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs pour chaque paramètre qui sont appropriées pour votre déploiement.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Pour récupérer les informations d’identification que vous avez ajouté au contrôleur de réseau, tapez la commande suivante, puis appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs pour chaque paramètre qui sont appropriées pour votre déploiement.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Passez en revue la sortie des commandes, qui doit être similaire à l’exemple de sortie suivant.

    ```
    Tags                   :
    ResourceRef     : /credentials/cred1
    CreatedTime    : 1/1/0001 12:00:00 AM
    InstanceId        : e16ffe62-a701-4d31-915e-7234d4bc5a18
    Etag                  : W/"1ec59631-607f-4d3e-ac78-94b0822f3a9d"
    ResourceMetadata :
    ResourceId       : cred1
    Properties       : Microsoft.Windows.NetworkController.CredentialProperties
    ```

    > [!NOTE]
    > Lorsque vous exécutez le **Get-NetworkControllerCredential** commande, vous pouvez affecter la sortie de la commande à une variable à l’aide de l’opérateur point pour répertorier les propriétés, les informations d’identification. Par exemple, $cred. Propriétés.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau

Après avoir déployé le contrôleur de réseau, vous pouvez utiliser les commandes Windows PowerShell pour gérer et modifier votre déploiement. Voici quelques-unes des modifications que vous pouvez apporter à votre déploiement.

- Modifier les paramètres de l’application, de cluster et de nœud contrôleur de réseau

- Supprimer le cluster de contrôleur de réseau et l’application

- Gérer les nœuds de cluster de contrôleur de réseau, y compris l’ajout, la suppression, l’activation et désactivation des nœuds.

Le tableau suivant fournit que la syntaxe Windows PowerShell pour les commandes que vous pouvez utiliser pour accomplir ces tâches.

|Tâche|Command|Syntaxe|
|--------|-------|----------|
|Modifier les paramètres de cluster de contrôleur de réseau|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres d’application de contrôleur de réseau|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres de nœud de contrôleur de réseau|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres de diagnostic de contrôleur de réseau|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer l’application de contrôleur de réseau|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer le cluster de contrôleur de réseau|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Ajouter un nœud au cluster de contrôleur de réseau|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Désactiver un nœud de cluster de contrôleur de réseau|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Activer un nœud de cluster de contrôleur de réseau|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Supprimer un nœud de contrôleur de réseau d’un cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Les commandes Windows PowerShell pour le contrôleur réseau se trouvent dans la bibliothèque TechNet à [applets de commande de contrôleur de réseau](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Exemple de script de configuration contrôleur de réseau

L’exemple de script de configuration suivant montre comment créer un cluster de contrôleur de réseau à plusieurs nœuds et installer l’application de contrôleur de réseau. En outre, la variable $cert sélectionne un certificat à partir du magasin de certificats ordinateur local qui correspond à la chaîne de nom de sujet « networkController.contoso.com ».

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Étapes de post-déploiement pour les déploiements de non-Kerberos

Si vous n’utilisez pas Kerberos avec votre déploiement de contrôleur de réseau, vous devez déployer des certificats.

Pour plus d’informations, consultez [étapes de post-déploiement pour contrôleur de réseau](../technologies/network-controller/post-deploy-steps-nc.md).



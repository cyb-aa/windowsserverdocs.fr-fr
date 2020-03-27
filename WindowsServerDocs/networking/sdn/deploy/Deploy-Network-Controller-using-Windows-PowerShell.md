---
title: Déployer le contrôleur de réseau à l’aide de Windows PowerShell
description: Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur un ou plusieurs ordinateurs ou machines virtuelles qui exécutent Windows Server 2016.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: get-started-article
ms.assetid: 2448d381-55aa-4c14-997a-202c537c6727
ms.author: lizross
author: eross-msft
ms.date: 08/23/2018
ms.openlocfilehash: ee3aa93c02419667b05a987f548ef4d14285231d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313073"
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Déployer le contrôleur de réseau à l’aide de Windows PowerShell

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur une ou plusieurs machines virtuelles qui exécutent Windows Server 2016.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur les hôtes physiques. Pour déployer le contrôleur de réseau, vous devez installer le rôle de serveur contrôleur de réseau sur un ordinateur virtuel Hyper-V \(\) d’ordinateur virtuel installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les machines virtuelles sur trois hôtes Hyper\-V différents, vous devez activer les hôtes Hyper\-V pour la mise en réseau définie par logiciel \(SDN\) en ajoutant les ordinateurs hôtes au contrôleur de réseau à l’aide de la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous activez le Load Balancer logiciel SDN pour fonctionner. Pour plus d’informations, consultez [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Cette rubrique contient les sections suivantes.

- [Installer le rôle de serveur du contrôleur de réseau](#install-the-network-controller-server-role)

- [Configurer le cluster de contrôleur de réseau](#configure-the-network-controller-cluster)

- [Configuration de l’application de contrôleur de réseau](#configure-the-network-controller-application)

- [Validation du déploiement du contrôleur de réseau](#network-controller-deployment-validation)

- [Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau](#additional-windows-powershell-commands-for-network-controller)

- [Exemple de script de configuration de contrôleur de réseau](#sample-network-controller-configuration-script)

- [Étapes de la suite du déploiement pour les déploiements non-Kerberos](#post-deployment-steps-for-non-kerberos-deployments)

## <a name="install-the-network-controller-server-role"></a>Installer le rôle de serveur du contrôleur de réseau

Vous pouvez utiliser cette procédure pour installer le rôle de serveur du contrôleur de réseau sur une machine virtuelle \(\)d’ordinateur virtuel.

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur les hôtes physiques. Pour déployer le contrôleur de réseau, vous devez installer le rôle de serveur contrôleur de réseau sur un ordinateur virtuel Hyper-V \(\) d’ordinateur virtuel installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les machines virtuelles sur trois hôtes Hyper\-V différents, vous devez activer les hôtes Hyper\-V pour la mise en réseau définie par logiciel \(la\) SDN en ajoutant les ordinateurs hôtes au contrôleur de réseau. En procédant ainsi, vous activez le Load Balancer logiciel SDN pour fonctionner.

Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Administrateurs** ou à un groupe équivalent.  

>[!NOTE]
>Si vous souhaitez utiliser Gestionnaire de serveur au lieu de Windows PowerShell pour installer le contrôleur de réseau, consultez [installer le rôle serveur contrôleur de réseau à l’aide de gestionnaire de serveur](https://technet.microsoft.com/library/mt403348.aspx)

Pour installer le contrôleur de réseau à l’aide de Windows PowerShell, tapez les commandes suivantes dans une invite Windows PowerShell, puis appuyez sur entrée.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

L’installation du contrôleur de réseau requiert le redémarrage de l’ordinateur. Pour ce faire, tapez la commande suivante, puis appuyez sur entrée.

`Restart-Computer`

## <a name="configure-the-network-controller-cluster"></a>Configurer le cluster de contrôleur de réseau

Le cluster de contrôleur de réseau offre une haute disponibilité et une évolutivité à l’application de contrôleur de réseau, que vous pouvez configurer après avoir créé le cluster et qui est hébergé sur le cluster.

>[!NOTE]
>Vous pouvez effectuer les procédures décrites dans les sections suivantes soit directement sur la machine virtuelle sur laquelle vous avez installé le contrôleur de réseau, soit vous pouvez utiliser la Outils d’administration de serveur distant pour Windows Server 2016 pour exécuter les procédures à partir d’un ordinateur distant qui exécute. Windows Server 2016 ou Windows 10. En outre, l’appartenance au **groupe administrateurs**, ou équivalent, est la condition minimale requise pour effectuer cette procédure. Si l’ordinateur ou la machine virtuelle sur lequel vous avez installé le contrôleur de réseau est joint à un domaine, votre compte d’utilisateur doit être membre des **utilisateurs du domaine**.

Vous pouvez créer un cluster de contrôleur de réseau en créant un objet de nœud, puis en configurant le cluster.

### <a name="create-a-node-object"></a>Créer un objet node

Vous devez créer un objet Node pour chaque machine virtuelle qui est membre du cluster de contrôleur de réseau.

Pour créer un objet node, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur entrée. Veillez à ajouter des valeurs pour chaque paramètre approprié pour votre déploiement.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la commande **New-NetworkControllerNodeObject** .

|Paramètre|Description|
|-------------|---------------|
|Nom|Le paramètre **Name** spécifie le nom convivial du serveur que vous souhaitez ajouter au cluster.|
|Serveur|Le paramètre **Server** spécifie le nom d’hôte, le nom de domaine complet (FQDN) ou l’adresse IP du serveur que vous souhaitez ajouter au cluster. Pour les ordinateurs joints à un domaine, un nom de domaine complet est requis.|
|FaultDomain|Le paramètre **FaultDomain** spécifie le domaine d’échec pour le serveur que vous ajoutez au cluster. Ce paramètre définit les serveurs qui peuvent rencontrer un échec en même temps que le serveur que vous ajoutez au cluster. Cet échec peut être dû à des dépendances physiques partagées, telles que des sources d’alimentation et de mise en réseau. Les domaines d’erreur représentent généralement des hiérarchies associées à ces dépendances partagées, avec un plus grand nombre de serveurs susceptibles de tomber en panne à partir d’un point supérieur dans l’arborescence de domaine d’erreur. Pendant l’exécution, le contrôleur de réseau prend en compte les domaines d’erreur dans le cluster et tente de répartir les services de contrôleur de réseau afin qu’ils se trouvent dans des domaines d’erreur distincts. Ce processus permet de s’assurer, en cas de défaillance d’un domaine d’erreur, que la disponibilité de ce service et de son état n’est pas compromise. Les domaines d’erreur sont spécifiés dans un format hiérarchique. Par exemple : « FD:/DC1/RACK1/host1 », où DC1 est le nom du centre de connaissances, RACK1 est le nom du rack et host1 est le nom de l’hôte sur lequel le nœud est placé.|
|RestInterface|Le paramètre **RestInterface** spécifie le nom de l’interface sur le nœud où la communication Rest est arrêtée. Cette interface de contrôleur de réseau reçoit les demandes de l’API Northbound à partir de la couche de gestion du réseau.|
|NodeCertificate|Le paramètre **NodeCertificate** spécifie le certificat utilisé par le contrôleur de réseau pour l’authentification de l’ordinateur. Le certificat est requis si vous utilisez l’authentification basée sur les certificats pour la communication au sein du cluster. le certificat est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Le nom d’objet du certificat doit être le même que le nom DNS du nœud.|

### <a name="configure-the-cluster"></a>Configurer le cluster

Pour configurer le cluster, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur entrée. Veillez à ajouter des valeurs pour chaque paramètre approprié pour votre déploiement.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la commande **install-NetworkControllerCluster** .
  
|Paramètre|Description|
|-------------|---------------|
|ClusterAuthentication|Le paramètre **ClusterAuthentication** spécifie le type d’authentification utilisé pour sécuriser la communication entre les nœuds et est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **x509** et **None**. L’authentification Kerberos utilise des comptes de domaine et peut uniquement être utilisée si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur x509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez configurer manuellement le certificat avant d’exécuter cette commande.|
|ManagementSecurityGroup|Le paramètre **ManagementSecurityGroup** spécifie le nom du groupe de sécurité qui contient les utilisateurs autorisés à exécuter les applets de commande de gestion à partir d’un ordinateur distant. Cela s’applique uniquement si ClusterAuthentication est Kerberos. Vous devez spécifier un groupe de sécurité de domaine et non un groupe de sécurité sur l’ordinateur local.|
|Nœud|Le paramètre **node** spécifie la liste des nœuds de contrôleur de réseau que vous avez créés à l’aide de la commande **New-NetworkControllerNodeObject** .|
|DiagnosticLogLocation|Le paramètre **DiagnosticLogLocation** spécifie l’emplacement du partage dans lequel les journaux de diagnostic sont régulièrement chargés. Si vous ne spécifiez pas de valeur pour ce paramètre, les journaux sont stockés localement sur chaque nœud. Les journaux sont stockés localement dans le dossier%systemdrive%\Windows\tracing\SDNDiagnostics. Les journaux de cluster sont stockés localement dans le dossier%systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|Le paramètre **LogLocationCredential** spécifie les informations d’identification requises pour accéder à l’emplacement de partage où les journaux sont stockés.|
|CredentialEncryptionCertificate|Le paramètre **CredentialEncryptionCertificate** spécifie le certificat utilisé par le contrôleur de réseau pour chiffrer les informations d’identification utilisées pour accéder aux fichiers binaires du contrôleur de réseau et au **LogLocationCredential**, s’ils sont spécifiés. Le certificat doit être approvisionné sur tous les nœuds du contrôleur de réseau avant d’exécuter cette commande, et le même certificat doit être inscrit sur tous les nœuds du cluster. L’utilisation de ce paramètre pour protéger les fichiers binaires et les journaux du contrôleur de réseau est recommandée dans les environnements de production. Sans ce paramètre, les informations d’identification sont stockées en texte clair et peuvent être utilisées de façon inutilisée par un utilisateur non autorisé.|
|Informations d'identification|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **Credential** spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **CertificateThumbprint** spécifie le certificat de clé publique numérique (x509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **UseSSL** spécifie le protocole protocole SSL (SSL) utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n'est pas utilisé.|
|ComputerName|Le paramètre **ComputerName** spécifie le nœud de contrôleur de réseau sur lequel cette commande est exécutée. Si vous ne spécifiez pas de valeur pour ce paramètre, l’ordinateur local est utilisé par défaut.|
|LogSizeLimitInMBs|Ce paramètre spécifie la taille maximale du journal, en Mo, que le contrôleur de réseau peut stocker. Les journaux sont stockés de manière circulaire. Si DiagnosticLogLocation est fourni, la valeur par défaut de ce paramètre est 40 Go. Si DiagnosticLogLocation n’est pas fourni, les journaux sont stockés sur les nœuds du contrôleur de réseau et la valeur par défaut de ce paramètre est 15 Go.|
|LogTimeLimitInDays|Ce paramètre spécifie la limite de durée, en jours, pendant laquelle les journaux sont stockés. Les journaux sont stockés de manière circulaire. La valeur par défaut de ce paramètre est 3 jours.|

## <a name="configure-the-network-controller-application"></a>Configuration de l’application de contrôleur de réseau
Pour configurer l’application de contrôleur de réseau, tapez la commande suivante à l’invite de commandes Windows PowerShell, puis appuyez sur entrée. Veillez à ajouter des valeurs pour chaque paramètre approprié pour votre déploiement.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la commande **install-NetworkController** .

|Paramètre|Description|
|-------------|---------------|
|ClientAuthentication|Le paramètre **clientauthentication** spécifie le type d’authentification utilisé pour sécuriser la communication entre REST et le contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **x509** et **None**. L’authentification Kerberos utilise des comptes de domaine et peut uniquement être utilisée si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur x509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez configurer manuellement le certificat avant d’exécuter cette commande.|
|Nœud|Le paramètre **node** spécifie la liste des nœuds de contrôleur de réseau que vous avez créés à l’aide de la commande **New-NetworkControllerNodeObject** .|
|ClientCertificateThumbprint|Ce paramètre est requis uniquement lorsque vous utilisez l’authentification basée sur les certificats pour les clients du contrôleur de réseau. Le paramètre **ClientCertificateThumbprint** spécifie l’empreinte numérique du certificat qui est inscrit aux clients sur la couche Northbound.|
|ServerCertificate|Le paramètre **serverCertificate** spécifie le certificat utilisé par le contrôleur de réseau pour prouver son identité aux clients. Le certificat de serveur doit inclure le rôle d’authentification du serveur dans les extensions d’utilisation améliorée de la clé et doit être délivré au contrôleur de réseau par une autorité de certification approuvée par les clients.|
|RESTIPAddress|Vous n’avez pas besoin de spécifier une valeur pour **RESTIPAddress** avec un déploiement à un seul nœud du contrôleur de réseau. Pour les déploiements à plusieurs nœuds, le paramètre **RESTIPAddress** spécifie l’adresse IP du point de terminaison REST en notation CIDR. Par exemple, 192.168.1.10/24. La valeur du nom d’objet de **serverCertificate** doit correspondre à la valeur du paramètre **RESTIPAddress** . Ce paramètre doit être spécifié pour tous les déploiements de contrôleur de réseau à plusieurs nœuds lorsque tous les nœuds se trouvent sur le même sous-réseau. Si les nœuds se trouvent sur des sous-réseaux différents, vous devez utiliser le paramètre **restname que** au lieu d’utiliser **RESTIPAddress**.|
|Restname que|Vous n’avez pas besoin de spécifier une valeur pour **restname que** avec un déploiement à un seul nœud du contrôleur de réseau. La seule fois où vous devez spécifier une valeur pour **restname que** est lorsque les déploiements à plusieurs nœuds ont des nœuds qui se trouvent sur des sous-réseaux différents. Pour les déploiements à plusieurs nœuds, le paramètre **restname que** spécifie le nom de domaine complet du cluster de contrôleur de réseau.|
|ClientSecurityGroup|Le paramètre **ClientSecurityGroup** spécifie le nom du groupe de sécurité Active Directory dont les membres sont des clients de contrôleur de réseau. Ce paramètre est obligatoire uniquement si vous utilisez l’authentification Kerberos pour **clientauthentication**. Le groupe de sécurité doit contenir les comptes à partir desquels les API REST sont accessibles, et vous devez créer le groupe de sécurité et ajouter des membres avant d’exécuter cette commande.|
|Informations d'identification|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **Credential** spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **CertificateThumbprint** spécifie le certificat de clé publique numérique (x509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le paramètre **UseSSL** spécifie le protocole protocole SSL (SSL) utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n'est pas utilisé.|

Une fois la configuration de l’application de contrôleur de réseau terminée, le déploiement du contrôleur de réseau est terminé.

## <a name="network-controller-deployment-validation"></a>Validation du déploiement du contrôleur de réseau

Pour valider votre déploiement de contrôleur de réseau, vous pouvez ajouter des informations d’identification au contrôleur de réseau, puis récupérer les informations d’identification.

Si vous utilisez Kerberos comme mécanisme ClientAuthentication, l’appartenance au groupe de **ClientSecurityGroup** que vous avez créé est le minimum requis pour effectuer cette procédure.

**Procédures**

1.  Sur un ordinateur client, si vous utilisez Kerberos comme mécanisme ClientAuthentication, connectez-vous avec un compte d’utilisateur qui est membre de votre **ClientSecurityGroup**.

2. Ouvrez Windows PowerShell, tapez les commandes suivantes pour ajouter des informations d’identification au contrôleur de réseau, puis appuyez sur entrée. Veillez à ajouter des valeurs pour chaque paramètre approprié pour votre déploiement.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Pour récupérer les informations d’identification que vous avez ajoutées au contrôleur de réseau, tapez la commande suivante, puis appuyez sur entrée. Veillez à ajouter des valeurs pour chaque paramètre approprié pour votre déploiement.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Examinez la sortie de la commande, qui doit ressembler à l’exemple de sortie suivant.

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
    > Quand vous exécutez la commande **NetworkControllerCredential** , vous pouvez affecter la sortie de la commande à une variable à l’aide de l’opérateur point pour répertorier les propriétés des informations d’identification. Par exemple, $cred. Sous.

## <a name="additional-windows-powershell-commands-for-network-controller"></a>Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau

Après avoir déployé le contrôleur de réseau, vous pouvez utiliser les commandes Windows PowerShell pour gérer et modifier votre déploiement. Voici quelques-unes des modifications que vous pouvez apporter à votre déploiement.

- Modifier les paramètres du nœud du contrôleur de réseau, du cluster et de l’application

- Supprimer le cluster de contrôleur de réseau et l’application

- Gérer les nœuds de cluster de contrôleur de réseau, notamment ajouter, supprimer, activer et désactiver des nœuds.

Le tableau suivant fournit la syntaxe des commandes Windows PowerShell que vous pouvez utiliser pour accomplir ces tâches.

|Tâche|Commande|Syntaxe|
|--------|-------|----------|
|Modifier les paramètres de cluster du contrôleur de réseau|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres d’application du contrôleur de réseau|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres du nœud du contrôleur de réseau|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres de diagnostic du contrôleur de réseau|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer l’application de contrôleur de réseau|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer le cluster de contrôleur de réseau|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Ajouter un nœud au cluster de contrôleur de réseau|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Désactiver un nœud de cluster de contrôleur de réseau|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Activer un nœud de cluster de contrôleur de réseau|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Supprimer un nœud de contrôleur de réseau d’un cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Les commandes Windows PowerShell pour le contrôleur de réseau se trouvent dans la bibliothèque TechNet au niveau des [applets de commande du contrôleur de réseau](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="sample-network-controller-configuration-script"></a>Exemple de script de configuration de contrôleur de réseau

L’exemple de script de configuration suivant montre comment créer un cluster de contrôleur de réseau à plusieurs nœuds et installer l’application de contrôleur de réseau. En outre, la variable $cert sélectionne un certificat dans le magasin de certificats de l’ordinateur local qui correspond à la chaîne du nom d’objet « networkController.contoso.com ».

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="post-deployment-steps-for-non-kerberos-deployments"></a>Étapes de la suite du déploiement pour les déploiements non-Kerberos

Si vous n’utilisez pas Kerberos avec votre déploiement de contrôleur de réseau, vous devez déployer des certificats.

Pour plus d’informations, consultez [étapes de publication du contrôleur de réseau](../technologies/network-controller/post-deploy-steps-nc.md).



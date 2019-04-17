---
title: Déployer le contrôleur de réseau à l’aide de Windows PowerShell
description: Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur un ou plusieurs ordinateurs ou des machines virtuelles (VM) qui exécutent Windows Server2016.
manager: brianlic
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
ms.openlocfilehash: cfd06662f317381fb77bf31db5ed60c2489ff871
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-network-controller-using-windows-powershell"></a>Déployer le contrôleur de réseau à l’aide de Windows PowerShell

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique fournit des instructions sur l’utilisation de Windows PowerShell pour déployer un contrôleur de réseau sur un ou plusieurs machines virtuelles (VM qui exécutent Windows Server2016).

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(VM\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les ordinateurs virtuels sur trois différents hôtes Hyper\-V, vous devez activer les ordinateurs hôtes Hyper\-V pour \(SDN\) logiciel défini de mise en réseau en ajoutant les ordinateurs hôtes à un contrôleur de réseau à l’aide de la commande Windows PowerShell **New-NetworkControllerServer**. En procédant ainsi, vous activez l’équilibrage de charge logicielle SDN de la fonction. Pour plus d’informations, voir [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Cette rubrique contient les sections suivantes.

- [Installer le rôle de serveur de contrôleur de réseau](#bkmk_role)

- [Configurer le contrôleur de réseau de cluster](#bkmk_configure)

- [Configurer l’application de contrôleur de réseau](#bkmk_app)

- [Validation de déploiement de contrôleur de réseau](#bkmk_validation)

- [Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau](#bkmk_ps)

- [Exemple de script de configuration contrôleur de réseau](#bkmk_script)

- [Post-Deployment après Étapes pour les déploiements Non Kerberos](#bkmk_nonkerb)

## <a name="bkmk_role"></a>Installer le rôle de serveur de contrôleur de réseau

Vous pouvez utiliser cette procédure pour installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel \(VM\).

>[!IMPORTANT]
>Ne déployez pas le rôle de serveur de contrôleur de réseau sur des hôtes physiques. Pour déployer un contrôleur de réseau, vous devez installer le rôle de serveur de contrôleur de réseau sur un ordinateur virtuel Hyper-V \(VM\) qui est installé sur un ordinateur hôte Hyper-V. Une fois que vous avez installé le contrôleur de réseau sur les ordinateurs virtuels sur trois différents hôtes Hyper\-V, vous devez activer les ordinateurs hôtes Hyper\-V pour \(SDN\) logiciel défini de mise en réseau en ajoutant les ordinateurs hôtes à un contrôleur de réseau. En procédant ainsi, vous activez l’équilibrage de charge logicielle SDN de la fonction.

L’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure.  

>[!NOTE]
>Si vous souhaitez utiliser le Gestionnaire de serveur au lieu de Windows PowerShell pour installer le contrôleur de réseau, consultez [installer le rôle de serveur de contrôleur de réseau à l’aide du Gestionnaire de serveur](https://technet.microsoft.com/library/mt403348.aspx)

Pour installer le contrôleur de réseau à l’aide de Windows PowerShell, tapez les commandes suivantes à une invite de commandes Windows PowerShell et appuyez sur ENTRÉE.

`Install-WindowsFeature -Name NetworkController -IncludeManagementTools`

Installation du contrôleur de réseau nécessite que vous redémarrez l’ordinateur. Pour ce faire, tapez la commande suivante et appuyez sur ENTRÉE.

`Restart-Computer`

## <a name="bkmk_configure"></a>Configurer le contrôleur de réseau de cluster

Le cluster de contrôleur de réseau fournit une haute disponibilité et évolutivité à l’application de contrôleur de réseau, vous pouvez configurer après la création du cluster, et qui est hébergé sur le cluster.

>[!NOTE]
>Vous pouvez effectuer les procédures décrites dans les sections suivantes directement sur l’ordinateur virtuel sur lequel vous avez installé le contrôleur de réseau, ou vous pouvez utiliser Remote Server Administration outils pour Windows Server2016 pour effectuer les procédures à partir d’un ordinateur distant qui exécute Windows Server2016 ou Windows10. En outre, l’appartenance au groupe **administrateurs**, ou équivalente, est la condition minimale requise pour effectuer cette procédure. Si l’ordinateur ou un ordinateur virtuel sur lequel vous avez installé le contrôleur de réseau est joint à un domaine, votre compte d’utilisateur doit être membre du **utilisateurs du domaine**.

Vous pouvez créer un cluster de contrôleur de réseau en créant un objet de nœud, puis la configuration du cluster.

### <a name="create-a-node-object"></a>Créer un objet de nœud

Vous devez créer un objet de nœud pour chaque ordinateur virtuel qui est membre du cluster de contrôleur de réseau.

Pour créer un objet de nœud, tapez la commande suivante à l’invite de commandes Windows PowerShell et appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs appropriées pour votre déploiement pour chaque paramètre.  

```
New-NetworkControllerNodeObject -Name <string> -Server <String> -FaultDomain <string>-RestInterface <string> [-NodeCertificate <X509Certificate2>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **New-NetworkControllerNodeObject** commande.

|Paramètre|Description|
|-------------|---------------|
|Nom|Le **nom** paramètre spécifie le nom convivial du serveur que vous souhaitez ajouter au cluster|
|Serveur|Le **Server** paramètre spécifie le nom d’hôte, le nom de domaine complet (FQDN) ou l’adresse IP du serveur que vous souhaitez ajouter au cluster. Pour les ordinateurs joints au domaine, le nom de domaine complet est requis.|
|FaultDomain|Le **FaultDomain** paramètre spécifie le domaine de l’échec du serveur que vous ajoutez au cluster. Ce paramètre définit les serveurs qui peuvent rencontrer échec en même temps que le serveur que vous ajoutez au cluster. Cet échec peut être en raison des dépendances physiques partagées telles que l’alimentation et les sources de mise en réseau. Domaines d’erreur représentent généralement les hiérarchies qui sont liées à ces dépendances partagés, avec des serveurs risques d’échouer ensemble d’un point plus élevé dans l’arborescence de domaine d’erreur. Pendant l’exécution, le contrôleur de réseau prend en compte les domaines d’erreur dans le cluster et tente de se propager les services de contrôleur de réseau afin qu’ils se trouvent dans des domaines d’erreur distincts. Ce processus permet de garantir, en cas de défaillance de n’importe quel domaine d’une défaillance, que la disponibilité de ce service et son état n’est pas compromise. Domaines d’erreur sont spécifiés dans un format hiérarchique. Par exemple: «Fd: / DC1/1/Host1», où DC1 est le nom du centre de données, 1 est le nom de rack et Host1 est le nom de l’ordinateur hôte où se trouve le nœud.|
|RestInterface|Le **RestInterface** paramètre spécifie le nom de l’interface sur le nœud où la communication Representational State Transfer (REST) est terminée. Cette interface de contrôleur de réseau reçoit les demandes de l’API Northbound à partir de la couche de gestion du réseau.|
|NodeCertificate|Le **NodeCertificate** paramètre spécifie le certificat que le contrôleur de réseau utilise pour l’authentification d’ordinateur. Le certificat est requis si vous utilisez l’authentification par certificat pour la communication au sein du cluster; le certificat est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Le nom de sujet du certificat doit être identique au nom DNS du nœud.|

### <a name="configure-the-cluster"></a>Configurer le cluster

Pour configurer le cluster, tapez la commande suivante à l’invite de commandes Windows PowerShell et appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs appropriées pour votre déploiement pour chaque paramètre.

```
Install-NetworkControllerCluster -Node <NetworkControllerNode[]> -ClusterAuthentication <ClusterAuthentication> [-ManagementSecurityGroup <string>][-DiagnosticLogLocation <string>][-LogLocationCredential <PSCredential>] [-CredentialEncryptionCertificate <X509Certificate2>][-Credential <PSCredential>][-CertificateThumbprint <String>] [-UseSSL][-ComputerName <string>][-LogSizeLimitInMBs<UInt32>] [-LogTimeLimitInDays<UInt32>]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **Install-NetworkControllerCluster** commande.
  
|Paramètre|Description|
|-------------|---------------|
|ClusterAuthentication|Le **ClusterAuthentication** paramètre spécifie le type d’authentification qui est utilisé pour sécuriser la communication entre les nœuds et est également utilisé pour le chiffrement du trafic entre les services de contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **X509** et **aucun**. L’authentification Kerberos utilise les comptes de domaine et peut être utilisée uniquement si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur les X509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez fournir manuellement le certificat avant d’exécuter cette commande.|
|ManagementSecurityGroup|Le **ManagementSecurityGroup** paramètre spécifie le nom du groupe de sécurité contenant les utilisateurs sont autorisés à exécuter les applets de commande de gestion à partir d’un ordinateur distant. Cela s’applique uniquement si ClusterAuthentication est Kerberos. Vous devez spécifier un groupe de sécurité de domaine et non un groupe de sécurité sur l’ordinateur local.|
|Nœud|Le **nœud** paramètre spécifie la liste des nœuds de contrôleur de réseau que vous avez créé à l’aide de la **New-NetworkControllerNodeObject** commande.|
|DiagnosticLogLocation|Le **DiagnosticLogLocation** paramètre spécifie l’emplacement du partage dans lequel les journaux de diagnostic sont régulièrement téléchargées. Si vous ne spécifiez pas une valeur pour ce paramètre, les journaux sont stockés localement sur chaque nœud. Les journaux sont stockés localement dans les systemdrive%\Windows\tracing\SDNDiagnostics % dossier. Journaux de cluster sont stockés localement dans le dossier de %systemdrive%\ProgramData\Microsoft\Service Fabric\log\Traces.|
|LogLocationCredential|Le **LogLocationCredential** paramètre spécifie les informations d’identification qui sont nécessaires pour accéder à l’emplacement du partage dans lequel les journaux sont stockés.|
|CredentialEncryptionCertificate|Le **CredentialEncryptionCertificate** paramètre spécifie le certificat que le contrôleur de réseau utilise pour chiffrer les informations d’identification qui sont utilisées pour accéder aux fichiers binaires du contrôleur de réseau et le **LogLocationCredential**, si spécifié. Le certificat doit être configuré sur tous les nœuds de contrôleur de réseau avant d’exécuter cette commande, et le même certificat doit être inscrit sur tous les nœuds du cluster. Pour protéger les journaux et les fichiers binaires du contrôleur de réseau à l’aide de ce paramètre est recommandé dans les environnements de production. Sans ce paramètre, les informations d’identification sont stockées en texte clair et peuvent être mal utilisées par tout utilisateur non autorisé.|
|Informations d’identification|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **Credential** paramètre spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **CertificateThumbprint** paramètre spécifie le numérique certificat de clé publique (X509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **UseSSL** paramètre spécifie le protocole Secure Sockets Layer (SSL) qui est utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n’est pas utilisé.|
|Nom_Ordinateur|Le **Nom_Ordinateur** paramètre spécifie le nœud du contrôleur de réseau sur lequel cette commande est exécuté. Si vous ne spécifiez pas une valeur pour ce paramètre, l’ordinateur local est utilisé par défaut.|
|LogSizeLimitInMBs|Ce paramètre spécifie la taille maximale du journal, en Mo, que le contrôleur de réseau peut stocker. Journaux sont stockés de façon circulaire. Si DiagnosticLogLocation est fournie, la valeur par défaut de ce paramètre est 40Go. Si DiagnosticLogLocation n’est pas fournie, les journaux sont stockés sur les nœuds de contrôleur de réseau et la valeur par défaut de ce paramètre est de 15Go.|
|LogTimeLimitInDays|Ce paramètre spécifie la limite de durée, en jours, pour lesquels les journaux sont stockés. Journaux sont stockés de façon circulaire. La valeur par défaut de ce paramètre est de 3jours.|

## <a name="bkmk_app"></a>Configurer l’application de contrôleur de réseau
Pour configurer l’application de contrôleur de réseau, tapez la commande suivante à l’invite de commandes Windows PowerShell et appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs appropriées pour votre déploiement pour chaque paramètre.

```
Install-NetworkController -Node <NetworkControllerNode[]> -ClientAuthentication <ClientAuthentication>  [-ClientCertificateThumbprint <string[]>]  [-ClientSecurityGroup <string>] -ServerCertificate <X509Certificate2> [-RESTIPAddress <String>] [-RESTName <String>] [-Credential <PSCredential>][-CertificateThumbprint <String> ] [-UseSSL]
```

Le tableau suivant fournit des descriptions pour chaque paramètre de la **Install-NetworkController** commande.

|Paramètre|Description|
|-------------|---------------|
|ClientAuthentication|Le **ClientAuthentication** paramètre spécifie le type d’authentification qui est utilisé pour sécuriser les communications entre reste et contrôleur de réseau. Les valeurs prises en charge sont **Kerberos**, **X509** et **aucun**. L’authentification Kerberos utilise les comptes de domaine et peut être utilisée uniquement si les nœuds de contrôleur de réseau sont joints à un domaine. Si vous spécifiez l’authentification basée sur les X509, vous devez fournir un certificat dans l’objet NetworkControllerNode. En outre, vous devez fournir manuellement le certificat avant d’exécuter cette commande.|
|Nœud|Le **nœud** paramètre spécifie la liste des nœuds de contrôleur de réseau que vous avez créé à l’aide de la **New-NetworkControllerNodeObject** commande.|
|ClientCertificateThumbprint|Ce paramètre est requis uniquement lorsque vous utilisez l’authentification par certificat pour les clients du contrôleur de réseau. Le **ClientCertificateThumbprint** paramètre spécifie l’empreinte numérique du certificat inscrit auprès des clients sur la couche Northbound.|
|Certificats|Le **certificats** paramètre spécifie le certificat que le contrôleur de réseau utilise pour prouver son identité aux clients. Le certificat de serveur doit inclure l’objectif de l’authentification du serveur dans les extensions d’utilisation améliorée de la clé et doit être émis par une autorité de certification approuvée par les clients à un contrôleur de réseau.|
|RESTIPAddress|Vous n’avez pas besoin de spécifier une valeur pour **RESTIPAddress** avec un déploiement de nœud unique de contrôleur de réseau. Pour les déploiements de plusieurs nœuds, la **RESTIPAddress** paramètre spécifie l’adresse IP du point de terminaison reste dans la notation CIDR. Par exemple, 192.168.1.10/24. La valeur de nom de l’objet de **certificats** doivent correspondre à la valeur de la **RESTIPAddress** paramètre. Ce paramètre doit être spécifié pour tous les déploiements de contrôleur de réseau de plusieurs nœuds, lorsque tous les nœuds sont sur le même sous-réseau. Si les nœuds sont sur différents sous-réseaux, vous devez utiliser le **RestName** paramètre au lieu d’utiliser **RESTIPAddress**.|
|RestName|Vous n’avez pas besoin de spécifier une valeur pour **RestName** avec un déploiement de nœud unique de contrôleur de réseau. Le seul moment où vous devez spécifier une valeur pour **RestName** est lors de déploiements de plusieurs nœuds disposent de nœuds qui se trouvent sur des sous-réseaux différents. Pour les déploiements de plusieurs nœuds, la **RestName** paramètre spécifie le nom de domaine complet pour le cluster de contrôleur de réseau.|
|ClientSecurityGroup|Le **ClientSecurityGroup** paramètre spécifie le nom du groupe de sécurité ActiveDirectory dont les membres sont des clients de contrôleur de réseau. Ce paramètre est requis uniquement si vous utilisez l’authentification Kerberos pour **ClientAuthentication**. Le groupe de sécurité doit contenir les comptes d’accès à partir de laquelle l’API REST, et vous devez créer le groupe de sécurité et ajouter des membres avant d’exécuter cette commande.|
|Informations d’identification|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **Credential** paramètre spécifie un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|CertificateThumbprint|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **CertificateThumbprint** paramètre spécifie le numérique certificat de clé publique (X509) d’un compte d’utilisateur qui a l’autorisation d’exécuter cette commande sur l’ordinateur cible.|
|UseSSL|Ce paramètre est obligatoire uniquement si vous exécutez cette commande à partir d’un ordinateur distant. Le **UseSSL** paramètre spécifie le protocole Secure Sockets Layer (SSL) qui est utilisé pour établir une connexion à l’ordinateur distant. Par défaut, SSL n’est pas utilisé.|

Après avoir terminé la configuration de l’application de contrôleur de réseau, votre déploiement de contrôleur de réseau est terminée.

## <a name="bkmk_validation"></a>Validation de déploiement de contrôleur de réseau

Pour valider votre déploiement de contrôleur de réseau, vous pouvez ajouter des informations d’identification sur le contrôleur de réseau et puis récupérer les informations d’identification.

Si vous utilisez Kerberos comme mécanisme ClientAuthentication, l’appartenance au groupe le **ClientSecurityGroup** que vous avez créé est la condition minimale requise pour effectuer cette procédure.

#### <a name="to-validate-deployment-of-network-controller"></a>Pour valider le déploiement de contrôleur de réseau

1.  Sur un ordinateur client, si vous utilisez Kerberos comme mécanisme ClientAuthentication, ouvrez une session avec un compte d’utilisateur qui est un membre de votre **ClientSecurityGroup**.

2. Ouvrez Windows PowerShell, tapez les commandes suivantes pour ajouter des informations d’identification au contrôleur de réseau et appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs appropriées pour votre déploiement pour chaque paramètre.

    ```
    $cred=New-Object Microsoft.Windows.Networkcontroller.credentialproperties
    $cred.type="usernamepassword"
    $cred.username="admin"
    $cred.value="abcd"

    New-NetworkControllerCredential -ConnectionUri https://networkcontroller -Properties $cred -ResourceId cred1
    ```

3. Pour récupérer les informations d’identification que vous avez ajouté au contrôleur de réseau, tapez la commande suivante et appuyez sur ENTRÉE. Assurez-vous que vous ajoutez les valeurs appropriées pour votre déploiement pour chaque paramètre.

    ```
    Get-NetworkControllerCredential -ConnectionUri https://networkcontroller -ResourceId cred1  
    ```

4. Examinez la sortie des commandes, qui doit être similaire à l’exemple de sortie suivant.

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
    > Lorsque vous exécutez le **Get-NetworkControllerCredential** de commande, vous pouvez affecter la sortie de la commande à une variable à l’aide de l’opérateur point pour répertorier les propriétés, les informations d’identification. Par exemple, $cred. Propriétés.

## <a name="bkmk_ps"></a>Commandes Windows PowerShell supplémentaires pour le contrôleur de réseau

Après avoir déployé le contrôleur de réseau, vous pouvez utiliser les commandes Windows PowerShell pour gérer et de modifier votre déploiement. Voici quelques-unes des modifications que vous pouvez apporter à votre déploiement.

- Modifier le contrôleur de réseau nœud, le cluster et les paramètres d’application

- Supprimer le cluster de contrôleur de réseau et l’application

- Gérer les nœuds de cluster de contrôleur de réseau, y compris l’ajout, suppression, l’activation et désactivation des nœuds.

Le tableau suivant fournit des que commandes de la syntaxe pour Windows PowerShell que vous pouvez utiliser pour effectuer ces tâches.

|Tâche|Commande|Syntaxe|
|--------|-------|----------|
|Modifier les paramètres du contrôleur de réseau de cluster|Set-NetworkControllerCluster|`Set-NetworkControllerCluster [-ManagementSecurityGroup <string>][-Credential <PSCredential>] [-computerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres d’application de contrôleur de réseau|Set-NetworkController|`Set-NetworkController [-ClientAuthentication <ClientAuthentication>] [-Credential <PSCredential>] [-ClientCertificateThumbprint <string[]>] [-ClientSecurityGroup <string>] [-ServerCertificate <X509Certificate2>] [-RestIPAddress <String>] [-ComputerName <String>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres du contrôleur de réseau de nœud|Set-NetworkControllerNode|`Set-NetworkControllerNode -Name <string> > [-RestInterface <string>] [-NodeCertificate <X509Certificate2>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Modifier les paramètres de diagnostic de contrôleur de réseau|Set-NetworkControllerDiagnostic|`Set-NetworkControllerDiagnostic [-LogScope <string>] [-DiagnosticLogLocation <string>] [-LogLocationCredential <PSCredential>] [-UseLocalLogLocation] >] [-LogLevel <loglevel>][-LogSizeLimitInMBs <uint32>] [-LogTimeLimitInDays <uint32>] [-Credential <PSCredential>] [-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer l’application de contrôleur de réseau|Uninstall-NetworkController|`Uninstall-NetworkController [-Credential <PSCredential>][-ComputerName <string>] [-CertificateThumbprint <String> ] [-UseSSL]`
|Supprimer le contrôleur de réseau de cluster|Uninstall-NetworkControllerCluster|`Uninstall-NetworkControllerCluster [-Credential <PSCredential>][-ComputerName <string>][-CertificateThumbprint <String> ] [-UseSSL]`
|Ajouter un nœud au cluster le contrôleur de réseau|Add-NetworkControllerNode|`Add-NetworkControllerNode -FaultDomain <String> -Name <String> -RestInterface <String> -Server <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-NodeCertificate <X509Certificate2> ] [-PassThru] [-UseSsl]`
|Désactiver un nœud de cluster du contrôleur de réseau|Disable-NetworkControllerNode|`Disable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Permettre à un nœud de cluster du contrôleur de réseau|Enable-NetworkControllerNode|`Enable-NetworkControllerNode -Name <String> [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-PassThru] [-UseSsl]`
|Suppression d’un nœud de contrôleur de réseau à partir d’un cluster|Remove-NetworkControllerNode|`Remove-NetworkControllerNode [-CertificateThumbprint <String> ] [-ComputerName <String> ] [-Credential <PSCredential> ] [-Force] [-Name <String> ] [-PassThru] [-UseSsl]`

>[!NOTE]
>Commandes Windows PowerShell pour le contrôleur de réseau se trouvent dans la bibliothèque TechNet à [applets de commande de contrôleur de réseau](https://technet.microsoft.com/library/mt576401.aspx).

## <a name="bkmk_script"></a>Exemple de script de configuration contrôleur de réseau

L’exemple de script de configuration suivant montre comment créer un cluster de contrôleur de réseau à plusieurs nœuds et installer l’application de contrôleur de réseau. En outre, la variable $cert sélectionne un certificat à partir du magasin de certificats ordinateur local qui correspond à la chaîne de nom d’objet «networkController.contoso.com».

```
$a = New-NetworkControllerNodeObject -Name Node1 -Server NCNode1.contoso.com -FaultDomain fd:/rack1/host1 -RestInterface Internal
$b = New-NetworkControllerNodeObject -Name Node2 -Server NCNode2.contoso.com -FaultDomain fd:/rack1/host2 -RestInterface Internal
$c = New-NetworkControllerNodeObject -Name Node3 -Server NCNode3.contoso.com -FaultDomain fd:/rack1/host3 -RestInterface Internal

$cert= get-item Cert:\LocalMachine\My | get-ChildItem | where {$_.Subject -imatch "networkController.contoso.com" }

Install-NetworkControllerCluster -Node @($a,$b,$c)  -ClusterAuthentication Kerberos -DiagnosticLogLocation \\share\Diagnostics - ManagementSecurityGroup Contoso\NCManagementAdmins -CredentialEncryptionCertificate $cert  
Install-NetworkController -Node @($a,$b,$c) -ClientAuthentication Kerberos -ClientSecurityGroup Contoso\NCRESTClients -ServerCertificate $cert -RestIpAddress 10.0.0.1/24
```

## <a name="bkmk_nonkerb"></a>Après le déploiement pour les étapes non - Kerberos déploiements

Si vous n’utilisez pas Kerberos avec votre déploiement de contrôleur de réseau, vous devez déployer des certificats.

Pour plus d’informations, voir [étapes de post-déploiement pour contrôleur réseau](../technologies/network-controller/post-deploy-steps-nc.md).



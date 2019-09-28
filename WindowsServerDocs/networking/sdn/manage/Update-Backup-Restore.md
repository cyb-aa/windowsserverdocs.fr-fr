---
title: Mettre à niveau, sauvegarder et restaurer l’infrastructure SDN
description: Dans cette rubrique, vous allez apprendre à mettre à jour, sauvegarder et restaurer une infrastructure SDN.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7f385e094ca70027d1b036bf53af23c1fc4a1bd1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406058"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Mettre à niveau, sauvegarder et restaurer l’infrastructure SDN

>S’applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à mettre à jour, sauvegarder et restaurer une infrastructure SDN. 

## <a name="upgrade-the-sdn-infrastructure"></a>Mettre à niveau l’infrastructure SDN
L’infrastructure SDN peut être mise à niveau de Windows Server 2016 vers Windows Server 2019. Pour le classement de mise à niveau, suivez la même séquence d’étapes que celle indiquée dans la section « mettre à jour l’infrastructure SDN ». Avant la mise à niveau, il est recommandé d’effectuer une sauvegarde de la base de données du contrôleur de réseau.

Pour les ordinateurs de contrôleur de réseau, utilisez l’applet de commande NetworkControllerNode pour vérifier l’état du nœud une fois la mise à niveau terminée. Assurez-vous que le nœud revient à l’État « haut » avant de mettre à niveau les autres nœuds. Une fois que vous avez mis à niveau tous les nœuds du contrôleur de réseau, le contrôleur de réseau met à jour les microservices qui s’exécutent au sein du cluster de contrôleur de réseau dans l’heure qui s’est écoulée. Vous pouvez déclencher une mise à jour immédiate à l’aide de l’applet de commande Update-networkcontroller. 

Installez les mêmes mises à jour Windows sur tous les composants du système d’exploitation du système SDN (Software Defined Networking), notamment :

- Ordinateurs hôtes Hyper-V activés pour SDN
- Machines virtuelles du contrôleur de réseau
- Logiciels Load Balancer les machines virtuelles MUX
- Machines virtuelles de passerelle RAS 

>[!IMPORTANT]
>Si vous utilisez System Center Virtual Manager, vous devez le mettre à jour avec les correctifs cumulatifs les plus récents.

Lorsque vous mettez à jour chaque composant, vous pouvez utiliser l’une des méthodes standard pour installer les mises à jour Windows. Toutefois, pour garantir un temps d’arrêt minimal pour les charges de travail et l’intégrité de la base de données du contrôleur de réseau, procédez comme suit :

1. Mettez à jour les consoles de gestion.<p>Installez les mises à jour sur chacun des ordinateurs sur lesquels vous utilisez le module PowerShell du contrôleur de réseau.  Y compris partout où le rôle RSAT-NetworkController est installé par lui-même. Exclusion des machines virtuelles du contrôleur de réseau elles-mêmes ; vous les mettez à jour à l’étape suivante.

2. Sur la première machine virtuelle du contrôleur de réseau, installez toutes les mises à jour et redémarrez.

3. Avant de passer à la machine virtuelle du contrôleur de réseau `get-networkcontrollernode` suivante, utilisez l’applet de commande pour vérifier l’état du nœud que vous avez mis à jour et redémarré.

4. Pendant le cycle de redémarrage, attendez que le nœud du contrôleur de réseau s’affiche, puis revenez en arrière.<p>Après le redémarrage de la machine virtuelle, plusieurs minutes peuvent être nécessaires avant de revenir à l’état **_d’avancement._** Pour obtenir un exemple de la sortie, consultez. 

5. Installez les mises à jour sur chaque machine virtuelle SLB à la fois pour garantir la disponibilité continue de l’infrastructure de l’équilibreur de charge.

6. Mettez à jour les hôtes Hyper-V et les passerelles RAS, en commençant par les ordinateurs hôtes qui contiennent les passerelles RAS en mode **veille** .<p>Les machines virtuelles de passerelle RAS ne peuvent pas être migrées en temps réel sans perdre les connexions clientes. Pendant le cycle de mise à jour, vous devez veiller à réduire le nombre de tentatives de basculement des connexions client vers une nouvelle passerelle RAS. En coordonnant la mise à jour des hôtes et des passerelles RAS, chaque locataire bascule une seule fois, au plus.

    a. Évacuez l’ordinateur hôte des machines virtuelles pouvant être migrées en direct.<p>Les machines virtuelles de la passerelle RAS doivent rester sur l’ordinateur hôte.

    b. Installez les mises à jour sur chaque machine virtuelle de passerelle sur cet ordinateur hôte.

    c. Si la mise à jour nécessite le redémarrage de la machine virtuelle de la passerelle, redémarrez la machine virtuelle.  

    d. Installez les mises à jour sur l’hôte contenant la machine virtuelle de passerelle qui vient d’être mise à jour.

    e. Redémarrez l’hôte si nécessaire par les mises à jour.

    f. Répétez cette opération pour chaque hôte supplémentaire contenant une passerelle de secours.<p>Si aucune passerelle de secours n’est conservée, suivez les mêmes étapes pour tous les hôtes restants.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Exemple : Utiliser l’applet de commande networkcontrollernode 

Dans cet exemple, vous voyez la sortie de l' `get-networkcontrollernode` applet de commande exécutée à partir de l’une des machines virtuelles du contrôleur de réseau.  

L’état des nœuds que vous voyez dans l’exemple de sortie est le suivant :

- NCNode1.contoso.com = en dessous
- NCNode2.contoso.com = haut
- NCNode3.contoso.com = haut

>[!IMPORTANT]
>Vous devez attendre quelques minutes jusqu’à ce que l’état du nœud soit _**modifié avant de mettre à jour**_ des nœuds supplémentaires, l’un après l’autre.

Une fois que vous avez mis à jour tous les nœuds du contrôleur de réseau, le contrôleur de réseau met à jour les microservices qui s’exécutent au sein du cluster de contrôleur de réseau dans l’heure qui s’est écoulée. 

>[!TIP]
>Vous pouvez déclencher une mise à jour immédiate `update-networkcontroller` à l’aide de l’applet de commande.


```Powershell
PS C:\> get-networkcontrollernode
Name            : NCNode1.contoso.com
Server          : NCNode1.Contoso.com
FaultDomain     : fd:/NCNode1.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Down

Name            : NCNode2.Contoso.com
Server          : NCNode2.contoso.com
FaultDomain     : fd:/ NCNode2.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up

Name            : NCNode3.Contoso.com
Server          : NCNode3.Contoso.com
FaultDomain     : fd:/ NCNode3.Contoso.com
RestInterface   : Ethernet
NodeCertificate :
Status          : Up
```

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Exemple : Utiliser l’applet de commande Update-networkcontroller
Dans cet exemple, vous voyez la sortie de l' `update-networkcontroller` applet de commande pour forcer la mise à jour du contrôleur de réseau. 

>[!IMPORTANT]
>Exécutez cette applet de commande lorsque vous n’avez plus de mises à jour à installer.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Sauvegarder l’infrastructure SDN

Les sauvegardes régulières de la base de données du contrôleur de réseau assurent la continuité des activités en cas de sinistre ou de perte de données.  La sauvegarde des machines virtuelles du contrôleur de réseau n’est pas suffisante, car elle ne garantit pas que la session continue sur les différents nœuds du contrôleur de réseau.

**Exigences**
* Un partage SMB et des informations d’identification avec des autorisations de lecture/écriture sur le partage et le système de fichiers.
* Vous pouvez éventuellement utiliser un compte de service administré de groupe (GMSA) si le contrôleur de réseau a été installé à l’aide d’un GMSA également.

**Procédures**

1. Utilisez la méthode de sauvegarde de la machine virtuelle de votre choix ou utilisez Hyper-V pour exporter une copie de chaque machine virtuelle du contrôleur de réseau.<p>La sauvegarde de la machine virtuelle du contrôleur de réseau garantit que les certificats nécessaires au déchiffrement de la base de données sont présents.  

2. Si vous utilisez System Center Virtual Machine Manager (SCVMM), arrêtez le service SCVMM et sauvegardez-le via SQL Server.<p>L’objectif ici est de s’assurer qu’aucune mise à jour n’est apportée à SCVMM pendant ce temps, ce qui peut créer une incohérence entre la sauvegarde du contrôleur de réseau et SCVMM.  

   >[!IMPORTANT]
   >Ne redémarrez pas le service SCVMM tant que la sauvegarde du contrôleur de réseau n’est pas terminée.

3. Sauvegardez la base de données du `new-networkcontrollerbackup` contrôleur de réseau avec l’applet de commande.

4. Vérifiez l’achèvement et la réussite de la sauvegarde avec `get-networkcontrollerbackup` l’applet de commande.

5. Si vous utilisez SCVMM, démarrez le service SCVMM.



### <a name="example-backing-up-the-network-controller-database"></a>Exemple : Sauvegarde de la base de données du contrôleur de réseau

```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

# Get or Create Credential object for File share user

$ShareUserResourceId = "BackupUser"

$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }
If ($ShareCredential -eq $null) {
    $CredentialProperties = New-Object Microsoft.Windows.NetworkController.CredentialProperties
    $CredentialProperties.Type = "usernamePassword"
    $CredentialProperties.UserName = "contoso\alyoung"
    $CredentialProperties.Value = "<Password>"

    $ShareCredential = New-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential -Properties $CredentialProperties -ResourceId $ShareUserResourceId -Force
}

# Create backup

$BackupTime = (get-date).ToString("s").Replace(":", "_")

$BackupProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerBackupProperties
$BackupProperties.BackupPath = "\\fileshare\backups\NetworkController\$BackupTime"
$BackupProperties.Credential = $ShareCredential

$Backup = New-NetworkControllerBackup -ConnectionURI $URI -Credential $Credential -Properties $BackupProperties -ResourceId $BackupTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Exemple : Vérification de l’état d’une opération de sauvegarde de contrôleur de réseau

```Powershell
PS C:\ > Get-NetworkControllerBackup -ConnectionUri $URI -Credential $Credential -ResourceId $Backup.ResourceId
| ConvertTo-JSON -Depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerBackup/2017-04-25T16_53_13",
    "InstanceId":  "c3ea75ae-2892-4e10-b26c-a2243b755dc8",
    "Etag":  "W/\"0dafea6c-39db-401b-bda5-d2885ded470e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-25T16_53_13",
    "Properties":  {
                    "BackupPath":  "\\\\fileshare\backups\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  "",
                    "FailedResourcesList":  [

                                            ],
                    "SuccessfulResourcesList":  [
                                                    "/networking/v1/credentials/11ebfc10-438c-4a96-a1ee-8a048ce675be",
                                                    "/networking/v1/credentials/41229069-85d4-4352-be85-034d0c5f4658",
                                                    "/networking/v1/credentials/b2a82c93-2583-4a1f-91f8-232b801e11bb",
                                                    "/networking/v1/credentials/BackupUser",
                                                    "/networking/v1/credentials/fd5b1b96-b302-4395-b6cd-ed9703435dd1",
                                                    "/networking/v1/virtualNetworkManager/configuration",
                                                    "/networking/v1/virtualSwitchManager/configuration",
                                                    "/networking/v1/accessControlLists/f8b97a4c-4419-481d-b757-a58483512640",
                                                    "/networking/v1/logicalnetworks/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8",
                                                    "/networking/v1/logicalnetworks/48610528-f40b-4718-938e-99c2be76f1e0",
                                                    "/networking/v1/logicalnetworks/89035b49-1ee3-438a-8d7a-f93cbae40619",
                                                    "/networking/v1/logicalnetworks/a9c8eaa0-519c-4988-acd6-11723e9efae5",
                                                    "/networking/v1/logicalnetworks/d4ea002c-c926-4c57-a178-461d5768c31f",
                                                    "/networking/v1/macPools/11111111-1111-1111-1111-111111111111",
                                                    "/networking/v1/loadBalancerManager/config",
                                                    "/networking/v1/publicIPAddresses/2c502b2d-b39a-4be1-a85a-55ef6a3a9a1d",
                                                    "/networking/v1/GatewayPools/Default",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8056-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5810-8057-b4c04f395931",
                                                    "/networking/v1/servers/4c4c4544-0058-5910-8056-b4c04f395931",
                                                    "/networking/v1/networkInterfaces/058430d3-af43-4328-a440-56540f41da50",
                                                    "/networking/v1/networkInterfaces/08756090-6d55-4dec-98d5-80c4c5a47db8",
                                                    "/networking/v1/networkInterfaces/2175d74a-aacd-44e2-80d3-03f39ea3bc5d",
                                                    "/networking/v1/networkInterfaces/2400c2c3-2291-4b0b-929c-9bb8da55851a",
                                                    "/networking/v1/networkInterfaces/4c695570-6faa-4e4d-a552-0b36ed3e0962",
                                                    "/networking/v1/networkInterfaces/7e317638-2914-42a8-a2dd-3a6d966028d6",
                                                    "/networking/v1/networkInterfaces/834e3937-f43b-4d3c-88be-d79b04e63bce",
                                                    "/networking/v1/networkInterfaces/9d668fe6-b1c6-48fc-b8b1-b3f98f47d508",
                                                    "/networking/v1/networkInterfaces/ac4650ac-c3ef-4366-96e7-d9488fb661ba",
                                                    "/networking/v1/networkInterfaces/b9f23e35-d79e-495f-a1c9-fa626b85ae13",
                                                    "/networking/v1/networkInterfaces/fdd929f1-f64f-4463-949a-77b67fe6d048",
                                                    "/networking/v1/virtualServers/15a891ee-7509-4e1d-878d-de0cb4fa35fd",
                                                    "/networking/v1/virtualServers/57416993-b410-44fd-9675-727cd4e98930",
                                                    "/networking/v1/virtualServers/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a",
                                                    "/networking/v1/virtualServers/6c812217-5931-43dc-92a8-1da3238da893",
                                                    "/networking/v1/virtualServers/d78b7fa3-812d-4011-9997-aeb5ded2b431",
                                                    "/networking/v1/virtualServers/d90820a5-635b-4016-9d6f-bf3f1e18971d",
                                                    "/networking/v1/loadBalancerMuxes/5f8aebdc-ee5b-488f-ac44-dd6b57bd316a_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d78b7fa3-812d-4011-9997-aeb5ded2b431_suffix",
                                                    "/networking/v1/loadBalancerMuxes/d90820a5-635b-4016-9d6f-bf3f1e18971d_suffix",
                                                    "/networking/v1/Gateways/15a891ee-7509-4e1d-878d-de0cb4fa35fd_suffix",
                                                    "/networking/v1/Gateways/57416993-b410-44fd-9675-727cd4e98930_suffix",
                                                    "/networking/v1/Gateways/6c812217-5931-43dc-92a8-1da3238da893_suffix",
                                                    "/networking/v1/virtualNetworks/b3dbafb9-2655-433d-b47d-a0e0bbac867a",
                                                    "/networking/v1/virtualNetworks/d705968e-2dc2-48f2-a263-76c7892fb143",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.2",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.3",
                                                    "/networking/v1/loadBalancers/24fa1af9-88d6-4cdc-aba0-66e38c1a7bb8_10.127.132.4"
                                                ],
                    "InProgressResourcesList":  [

                                                ],
                    "ProvisioningState":  "Succeeded",
                    "Credential":  {
                                        "Tags":  null,
                                        "ResourceRef":  "/credentials/BackupUser",
                                        "InstanceId":  "00000000-0000-0000-0000-000000000000",
                                        "Etag":  null,
                                        "ResourceMetadata":  null,
                                        "ResourceId":  null,
                                        "Properties":  null
                                    }
                }
}
```

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Restauration de l’infrastructure SDN à partir d’une sauvegarde

Lorsque vous restaurez tous les composants nécessaires à partir de la sauvegarde, l’environnement SDN revient à un état opérationnel.  

>[!IMPORTANT]
>Les étapes varient en fonction du nombre de composants restaurés.


1. Si nécessaire, Redéployez les hôtes Hyper-V et le stockage nécessaire.

2. Si nécessaire, restaurez les machines virtuelles du contrôleur de réseau, les machines virtuelles de la passerelle RAS et les machines virtuelles MUX à partir de 

3. Arrêtez l’agent hôte NC et l’agent hôte SLB sur tous les ordinateurs hôtes Hyper-V :

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Arrêtez les machines virtuelles de passerelle RAS.

5. Arrêtez les machines virtuelles MUX SLB.

6. Restaurez le contrôleur de réseau `new-networkcontrollerrestore` à l’aide de l’applet de commande.

7. Vérifiez le **ProvisioningState** de restauration pour savoir quand la restauration s’est terminée avec succès.

8. Si vous utilisez SCVMM, restaurez la base de données SCVMM à l’aide de la sauvegarde qui a été créée en même temps que la sauvegarde du contrôleur de réseau.

9. Si vous souhaitez restaurer des machines virtuelles de charge de travail à partir d’une sauvegarde, faites-le maintenant.

10. Vérifiez l’intégrité de votre système avec l’applet de commande Debug-networkcontrollerconfigurationstate.

```Powershell
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController "https://NC.contoso.com" -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

### <a name="example-restoring-a-network-controller-database"></a>Exemple : Restauration d’une base de données de contrôleur de réseau
 
```Powershell
$URI = "https://NC.contoso.com"
$Credential = Get-Credential

$ShareUserResourceId = "BackupUser"
$ShareCredential = Get-NetworkControllerCredential -ConnectionURI $URI -Credential $Credential | Where {$_.ResourceId -eq $ShareUserResourceId }

$RestoreProperties = New-Object Microsoft.Windows.NetworkController.NetworkControllerRestoreProperties
$RestoreProperties.RestorePath = "\\fileshare\backups\NetworkController\2017-04-25T16_53_13"
$RestoreProperties.Credential = $ShareCredential

$RestoreTime = (Get-Date).ToString("s").Replace(":", "_")
New-NetworkControllerRestore -ConnectionURI $URI -Credential $Credential -Properties $RestoreProperties -ResourceId $RestoreTime -Force
```

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Exemple : Vérification de l’état d’une restauration de base de données de contrôleur de réseau

```PowerShell
PS C:\ > get-networkcontrollerrestore -connectionuri $uri -credential $cred -ResourceId $restoreTime | convertto-json -depth 10
{
    "Tags":  null,
    "ResourceRef":  "/networkControllerRestore/2017-04-26T15_04_44",
    "InstanceId":  "22edecc8-a613-48ce-a74f-0418789f04f6",
    "Etag":  "W/\"f14f6b84-80a7-4b73-93b5-59a9c4b5d98e\"",
    "ResourceMetadata":  null,
    "ResourceId":  "2017-04-26T15_04_44",
    "Properties":  {
                    "RestorePath":  "\\\\sa18fs\\sa18n22\\NetworkController\\2017-04-25T16_53_13",
                    "ErrorMessage":  null,
                    "FailedResourcesList":  null,
                    "SuccessfulResourcesList":  null,
                    "ProvisioningState":  "Succeeded",
                    "Credential":  null
                }
}
```


Pour plus d’informations sur les messages d’état de configuration qui peuvent s’afficher, consultez [résoudre les problèmes de la pile de mise en réseau définie par le logiciel Windows Server 2016](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).
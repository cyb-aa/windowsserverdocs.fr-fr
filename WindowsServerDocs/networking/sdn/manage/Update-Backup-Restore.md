---
title: Mise à niveau, la sauvegarde et restauration SDN infrastructure
description: Dans cette rubrique, vous allez apprendre à mettre à jour, de sauvegarde et de restauration d’une infrastructure SDN.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: shortpatti
ms.date: 08/27/2018
ms.openlocfilehash: 7916377f58261d0ccaa3fa24f135fccca3d5e79b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446335"
---
# <a name="upgrade-backup-and-restore-sdn-infrastructure"></a>Mise à niveau, la sauvegarde et restauration SDN infrastructure

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Dans cette rubrique, vous allez apprendre à mettre à jour, de sauvegarde et de restauration d’une infrastructure SDN. 

## <a name="upgrade-the-sdn-infrastructure"></a>Mise à niveau l’infrastructure SDN
Infrastructure SDN peut être mis à niveau à partir de Windows Server 2016 pour Windows Server 2019. Pour la mise à niveau de classement, suivez la même séquence d’étapes, comme indiqué dans la section « Mise à jour de l’infrastructure SDN ». Avant la mise à niveau, il est recommandé pour effectuer une sauvegarde de la base de données du contrôleur de réseau.

Pour les machines de contrôleur de réseau, utilisez l’applet de commande Get-NetworkControllerNode pour vérifier l’état du nœud après que la mise à niveau est terminée. Assurez-vous que le nœud est à nouveau « Jusqu'à « état avant la mise à niveau les autres nœuds. Une fois que vous avez mis à niveau tous les nœuds de contrôleur de réseau, le contrôleur de réseau met à jour les microservices en cours d’exécution au sein du cluster de contrôleur de réseau au sein d’une heure. Vous pouvez déclencher une mise à jour immédiate à l’aide de l’applet de commande update-networkcontroller. 

Installer les mises à jour de Windows mêmes sur tous les composants de système d’exploitation du système de mise en réseau SDN (Software Defined), qui inclut :

- SDN activé des hôtes Hyper-V
- Machines virtuelles du contrôleur réseau
- Machines virtuelles Mux d’équilibreur de charge logiciel
- Machines virtuelles de passerelle RAS 

>[!IMPORTANT]
>Si vous utilisez System Center Virtual Manager, vous devez le mettre à jour avec les dernières mises à jour cumulatives.

Lorsque vous mettez à jour chaque composant, vous pouvez utiliser une des méthodes standards pour l’installation des mises à jour de Windows. Toutefois, pour garantir des temps d’arrêt minimal pour les charges de travail et l’intégrité de la base de données du contrôleur de réseau, procédez comme suit :

1. Mettre à jour les consoles de gestion.<p>Installer les mises à jour sur chacun des ordinateurs où vous utilisez le module Powershell de contrôleur de réseau.  N’importe où, notamment d’avoir le rôle de RSAT-NetworkController installé par lui-même. À l’exclusion de la machines virtuelles du contrôleur réseau elles-mêmes. vous les mettre à jour à l’étape suivante.

2. Sur la première machine virtuelle du contrôleur de réseau, installez toutes les mises à jour, puis redémarrez.

3. Avant de passer à la machine virtuelle de contrôleur de réseau suivant, utilisez le `get-networkcontrollernode` applet de commande pour vérifier l’état du nœud que vous avez mis à jour et redémarré.

4. Au cours du cycle de redémarrage, attendez que le nœud de contrôleur de réseau à tomber en panne et puis revienne en ligne à nouveau.<p>Après le redémarrage de la machine virtuelle, il peut prendre plusieurs minutes avant qu’il réintègre le **_des_** état. Pour obtenir un exemple de la sortie, consultez 

5. Installer les mises à jour sur chaque machine virtuelle de SLB Mux un à la fois pour assurer une disponibilité permanente de l’infrastructure d’équilibrage de charge.

6. Mettre à jour les hôtes Hyper-V et les passerelles RAS, en commençant par les hôtes qui contiennent les passerelles RAS qui se trouvent dans **Standby** mode.<p>Machines virtuelles de passerelle RAS ne peut pas être migrées de façon dynamique sans perdre les connexions client. Au cours du cycle de mise à jour, vous devez veiller à réduire le nombre de fois locataire basculement de connexions à une passerelle RAS. Grâce à la coordination de la mise à jour des hôtes et des passerelles RAS, chaque client bascule une fois, au maximum.

    a. Évacuer l’hôte de machines virtuelles qui sont capables de migration en direct.<p>Machines virtuelles de passerelle RAS doivent rester sur l’ordinateur hôte.

    b. Installer les mises à jour sur chaque machine virtuelle de passerelle sur cet ordinateur hôte.

    c. Si la mise à jour nécessite le redémarrage de machine virtuelle de passerelle, puis redémarrez la machine virtuelle.  

    d. Installer les mises à jour sur l’ordinateur hôte contenant la machine virtuelle qui a été mis à jour de la passerelle.

    e. Redémarrez l’hôte si requis par les mises à jour.

    f. Répétez pour chaque hôte supplémentaire contenant une passerelle de secours.<p>Si aucune passerelle secours ne reste, puis suivez ces étapes pour tous les hôtes restants.


### <a name="example-use-the-get-networkcontrollernode-cmdlet"></a>Exemple : Utilisez l’applet de commande get-networkcontrollernode 

Dans cet exemple, vous voyez la sortie pour le `get-networkcontrollernode` applet de commande exécuté à partir d’une des machines virtuelles de contrôleur de réseau.  

L’état des nœuds que vous voyez dans l’exemple de sortie est :

- NCNode1.contoso.com = Down
- NCNode2.contoso.com = Up
- NCNode3.contoso.com = Up

>[!IMPORTANT]
>Vous devez attendre quelques minutes jusqu'à ce que l’état pour le nœud devient alors _**des**_ avant de vous mettre à jour tous les nœuds supplémentaires à la fois.

Une fois que vous avez mis à jour tous les nœuds de contrôleur de réseau, le contrôleur de réseau met à jour les microservices en cours d’exécution au sein du cluster de contrôleur de réseau au sein d’une heure. 

>[!TIP]
>Vous pouvez déclencher une mise à jour immédiate à l’aide de la `update-networkcontroller` applet de commande.


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

### <a name="example-use-the-update-networkcontroller-cmdlet"></a>Exemple : Utilisez l’applet de commande update-networkcontroller
Dans cet exemple, vous voyez la sortie pour le `update-networkcontroller` applet de commande pour forcer le contrôleur de réseau pour mettre à jour. 

>[!IMPORTANT]
>Exécutez cette applet de commande lorsque vous ne disposez d’aucun autres mises à jour à installer.


```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

## <a name="backup-the-sdn-infrastructure"></a>Sauvegarde l’infrastructure SDN

Des sauvegardes régulières de la base de données du contrôleur de réseau garantit la continuité d’activité en cas de perte de données ou d’urgence.  Sauvegarde des machines virtuelles de contrôleur de réseau n’est pas suffisante, car il ne garantit pas que la session se poursuit sur plusieurs nœuds de contrôleur de réseau.

**Configuration requise :**
* Un partage SMB et les informations d’identification avec des autorisations de lecture/écriture dans le système de partage et de fichiers.
* Vous pouvez éventuellement utiliser un compte de Service gérés groupe (GMSA) si le contrôleur de réseau a été installé à l’aide d’un compte GMSA également.

**Procédure :**

1. Utilisez la méthode de sauvegarde de machine virtuelle de votre choix, ou Hyper-V pour exporter une copie de chaque machine virtuelle contrôleur de réseau.<p>Sauvegarder la machine virtuelle contrôleur de réseau garantit que les certificats nécessaires pour déchiffrer la base de données sont présents.  

2. Si vous utilisez System Center Virtual Machine Manager (SCVMM), arrêtez le service SCVMM et sauvegarder via SQL Server.<p>L’objectif ici est pour vous assurer qu’aucune mise à jour n’effectuées à SCVMM pendant ce temps, ce qui peut entraîner une incohérence entre la sauvegarde de contrôleur de réseau et de SCVMM.  

   >[!IMPORTANT]
   >Ne ré-démarrez pas le service SCVMM jusqu'à ce que la sauvegarde de contrôleur de réseau est terminée.

3. Sauvegarder la base de données du contrôleur de réseau avec la `new-networkcontrollerbackup` applet de commande.

4. Vérifiez la saisie semi-automatique et la réussite de la sauvegarde avec la `get-networkcontrollerbackup` applet de commande.

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

### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Exemple : Vérification de l’état d’une opération de sauvegarde du contrôleur de réseau

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

## <a name="restore-the-sdn-infrastructure-from-a-backup"></a>Restaurer l’infrastructure SDN à partir d’une sauvegarde

Lorsque vous restaurez tous les composants nécessaires à partir de la sauvegarde, l’environnement SDN retourne à un état opérationnel.  

>[!IMPORTANT]
>Les étapes varient selon le nombre de composants restaurés.


1. Si nécessaire, redéployez les hôtes Hyper-V et le stockage nécessaire.

2. Si nécessaire, restaurez les machines virtuelles du contrôleur de réseau, les machines virtuelles de passerelle RAS et les machines virtuelles Mux à partir de la sauvegarde. 

3. Agent hôte de Stop NC et SLB hébergent l’agent sur tous les hôtes Hyper-V :

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Arrêter les machines virtuelles de passerelle RAS.

5. Arrêter les machines virtuelles SLB Mux.

6. Restaurer le contrôleur de réseau avec la `new-networkcontrollerrestore` applet de commande.

7. Vérifier la restauration **ProvisioningState** savoir quand la restauration s’est déroulée correctement.

8. Si vous utilisez SCVMM, restaurez la base de données SCVMM à l’aide de la sauvegarde a été créée en même temps que la sauvegarde de contrôleur de réseau.

9. Si vous souhaitez restaurer des machines virtuelles de charge de travail à partir de la sauvegarde, faites-le maintenant.

10. Vérifier l’intégrité de votre système avec l’applet de commande debug-networkcontrollerconfigurationstate.

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

### <a name="example-restoring-a-network-controller-database"></a>Exemple : Restauration d’une base de données du contrôleur de réseau
 
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

### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Exemple : Vérification de l’état d’une restauration de base de données du contrôleur de réseau

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


Pour plus d’informations sur les messages d’état de configuration susceptibles d’apparaître, consultez [résoudre les problèmes de la Windows Server 2016 défini par la mise en réseau pile logicielle](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).
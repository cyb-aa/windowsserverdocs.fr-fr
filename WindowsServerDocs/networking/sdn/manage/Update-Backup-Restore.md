---
title: "Mise à jour, la sauvegarde et restauration logicielle définie Infrastructure de mise en réseau"
description: "Cette rubrique fait partie du guide logiciel défini de mise en réseau sur l’infrastructure de mise à jour, sauvegarde et restauration de type SDN dans Windows Server2016."
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: e9a8f2fd-48fe-4a90-9250-f6b32488b7a4
ms.author: grcusanz
author: grcusanz
ms.openlocfilehash: bb7194ec865db980962853b87d68a84a5446269e
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/17/2017
---
# <a name="update-backup-and-restore-software-defined-networking-infrastructure"></a>Mise à jour, la sauvegarde et restauration logicielle définie Infrastructure de mise en réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Cette rubrique contient les sections suivantes.

- [Mise à jour de l’infrastructure SDN](#bkmk_Updating)
- [Sauvegarde de l’infrastructure SDN](#bkmk_backup)
- [Restauration de l’infrastructure SDN à partir d’une sauvegarde](#bkmk_restore)

## <a name="bkmk_Updating"></a>Mise à jour de l’Infrastructure SDN

Mise à jour est le processus d’installation des mises à jour Windows sur tous les composants du système d’exploitation du système logiciel défini de mise en réseau (SDN).  Cela inclut le SDN activé les hôtes Hyper-V, machines virtuelles de contrôleur de réseau, logiciel charge équilibrage Mux ordinateurs virtuels et ordinateurs virtuels de passerelle RAS.  Il est essentiel que tous ces composants ont exactement le même jeu de mises à jour installées.  Si vous utilisez SystemCenter Virtual Machine Manager. il est également recommandé que vous également à jour avec les dernières cumulatifs également.

Mise à jour de chaque composant est effectuée à l’aide d’une des méthodes standards pour l’installation des mises à jour windows, cependant les étapes décrites ci-dessous doivent être suivie afin d’assurer un minimum de temps pour les charges de travail et pour garantir l’intégrité de la base de données du contrôleur de réseau.

### <a name="step-1-update-the-management-consoles"></a>Étape1: Mettre à jour les consoles de gestion
Installer les mises à jour nécessaires sur chacun des ordinateurs où vous utilisez le module Powershell de contrôleur de réseau.  Cela inclut n’importe où que vous avez le rôle de RSAT-NetworkController installé par lui-même.  Cela ne pas y compris les ordinateurs virtuels en contrôleur de réseau eux-mêmes comme ils seront mis à jour à l’étape2.

### <a name="step-2-update-the-network-controllers"></a>Étape2: Mettre à jour les contrôleurs de réseau
Cela est l’étape la plus importante dans le cycle de mise à jour dans la mesure où chaque ordinateur de contrôleur de réseau virtuel doit être mis à jour et être entièrement nouveau en ligne dans le cluster de contrôleur de réseau avant de passer à la suivante.

Démarrez avec un ordinateur de contrôleur de réseau virtuel et installez toutes les mises à jour nécessaires.  Redémarrez l’ordinateur virtuel si nécessaire.

Avant de passer à la prochaine ordinateur virtuel du contrôleur de réseau utiliser get-networkcontrollernode pour vérifier l’état du nœud qui a été mis à jour et redémarré.  Attendez que le nœud du contrôleur de réseau accéder vers le bas au cours du cycle de redémarrage, puis revenez nouveau.  Une fois que l’ordinateur virtuel a redémarré, peut toujours prendre quelques minutes pour revenir en arrière dans l’état.

#### <a name="example-using-get-networkcontrollernode-to-check-the-status-of-network-controller-nodes"></a>Exemple: À l’aide de get-networkcontrollernode pour vérifier l’état des nœuds de contrôleur de réseau

Cet exemple montre la sortie de get-networkcontrollernode à partir d’un des ordinateurs virtuels contrôleur de réseau en cours d’exécution.  Il montre que NCNode1.contoso.com s’est arrêté alors que les deux nœuds sont sains.  Vous devez attendre jusqu'à plusieurs minutes jusqu'à ce que l’état pour ce nœud des modifications à configurer avant de procéder à la mise à jour de tous les nœuds supplémentaires.

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

Uniquement une fois tous les nœuds de contrôleur de réseau dans l’état, pouvez répéter ces étapes pour chaque nœud supplémentaire de contrôleur de réseau?  Continuer à mettre à jour de chaque nœud d’un à la fois.

Une fois que tous les nœuds de contrôleur de réseau sont mis à jour, le contrôleur de réseau met à jour le microservices en cours d’exécution au sein du cluster de contrôleur de réseau au sein d’une heure.  Vous pouvez déclencher une mise à jour immédiate, à l’aide de l’applet de commande update-networkcontroller.

#### <a name="example-using-update-networkcontroller-to-force-network-controller-to-update"></a>Exemple: À l’aide de mise à jour-networkcontroller pour forcer le contrôleur de réseau pour mettre à jour

Cette commande montre le résultat de la mise à jour-networkcontroller lorsqu’il n’y a pas restant à installer les mises à jour.

```Powershell
PS C:\> update-networkcontroller
NetworkControllerClusterVersion NetworkControllerVersion
------------------------------- ------------------------
10.1.1                          10.1.15
```

### <a name="step-3-update-slb-muxes"></a>Étape3: Mise à jour SLB Muxes

Installer les mises à jour sur chaque ordinateur virtuel Mux de SLB un à la fois pour garantir une disponibilité continue de l’infrastructure d’équilibrage de charge.

### <a name="step-4-update-hyper-v-hosts-and-ras-gateways"></a>Étape4: Mise à jour les ordinateurs hôtes Hyper-V et les passerelles RAS

Étant donné que les ordinateurs virtuels de passerelle RAS ne peuvent pas être migrées dynamiquement sans perdre les connexions clientes, il convient afin de réduire le nombre de fois que ce client connexions seront basculées vers une nouvelle passerelles RAS au cours du cycle de mise à jour.  Par la coordination de la mise à jour des ordinateurs hôtes et des passerelles RAS chaque client effectue uniquement un basculement au maximum une fois.  

Pour chaque hôte, en commençant par les ordinateurs hôtes qui contiennent les passerelles RAS qui sont en mode de veille, procédez comme suit:

1.  Déplacer l’ordinateur hôte d’ordinateurs virtuels qui sont capables de la migration dynamique.  Ordinateurs virtuels de passerelle RAS doit rester sur l’ordinateur hôte.
2.  Installer les mises à jour sur chaque ordinateur virtuel de passerelle sur cet ordinateur hôte.
3.  Si la mise à jour nécessite la passerelle ordinateur virtuel à redémarrer, puis redémarrez l’ordinateur virtuel.  
4.  Installer les mises à jour sur l’ordinateur hôte contenant l’ordinateur virtuel qui a été mis à jour de la passerelle.
5.  Redémarrez l’ordinateur hôte si requis par les mises à jour.
6.  Répétez pour chaque hôte supplémentaire contenant une passerelle de mise en veille.  Si aucune mise en veille passerelles ne restent, puis suivez ces étapes pour tous les hôtes restants.

## <a name="bkmk_backup"></a>Sauvegarde de l’infrastructure SDN

Des sauvegardes régulières de la base de données du contrôleur de réseau sont essentielles pour assurer la continuité d’activité en cas d’une perte de données ou de reprise après sinistre.  Sauvegarde d’ordinateurs virtuels contrôleur de réseau est insuffisante, car elle ne garantit pas que le quorum est conservé entre plusieurs nœuds de contrôleur de réseau.
Configuration requise:
 * Un partage SMB et les informations d’identification disposant des autorisations en lecture/écriture au système de partage et de fichiers.
 * Vous pouvez éventuellement utiliser un compte de Service géré groupe (GMSA) si le contrôleur de réseau a été installé à l’aide d’un compte GMSA également.

Suivez ces étapes pour effectuer une sauvegarde:

1. Les machines virtuelles de contrôleur de réseau à l’aide de la méthode de sauvegarde d’ordinateurs virtuels de votre choix de la sauvegarde, ou utilisez Hyper-V pour exporter une copie de chaque ordinateur de contrôleur de réseau virtuel.  Cela permet de garantir que si une reconstruction complète, y compris la restauration de l’infrastructure de machines virtuelles est effectuée, les certificats nécessaires pour le déchiffrement de la base de données sont présents.
2. Si vous utilisez SystemCenter Virtual Machine Manager (SCVMM), arrêtez le service SCVMM et sauvegarder via SQLServer pour vous assurer qu’aucune mise à jour n’est apportées à SCVMM pendant ce temps, ce qui peut entraîner une incohérence entre la sauvegarde du contrôleur de réseau et SCVMM.  Ne ré-démarrent pas le service SCVMM jusqu'à ce que la sauvegarde du contrôleur de réseau est terminée.
3. Sauvegarde la base de données du contrôleur de réseau à l’aide de la nouvelle networkcontrollerbackup.

 #### <a name="example-backing-up-the-network-controller-database"></a>Exemple: Sauvegarde de la base de données du contrôleur de réseau
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

4. Utilisez get-networkcontrollerbackup pour vérifier la réussite de la sauvegarde et l’achèvement.

 #### <a name="example-checking-the-status-of-a-network-controller-backup-operation"></a>Exemple: Vérification de l’état d’une opération de sauvegarde du contrôleur de réseau

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

5.  Si vous utilisez SCVMM vous pouvez maintenant démarrer le service SCVMM.

## <a name="bkmk_restore"></a>Restauration de l’infrastructure SDN à partir d’une sauvegarde

Restauration est le processus de restauration de tous les composants nécessaires à partir de la sauvegarde pour renvoyer un environnement SDN à un état opérationnel.  Les étapes varient légèrement en fonction de la quantité de composants qui sont en cours de restauration.

1. Si nécessaire, redéployer les hôtes Hyper-V et le stockage nécessaire.

2. Si nécessaire, vous devez restaurer les ordinateurs virtuels du contrôleur de réseau, les ordinateurs virtuels de passerelle RAS et les machines virtuelles Mux à partir de la sauvegarde. 

3. Arrêtez le NC hôte Agent et SLB hôte Agent sur tous les ordinateurs hôtes Hyper-V

    ```
    stop-service slbhostagent

    stop-service nchostagent
    ```

4. Arrêter les ordinateurs virtuels de passerelle RAS

5. Arrêter les ordinateurs virtuels SLB Mux

6. Restaurez le contrôleur de réseau à l’aide de l’applet de commande Nouveau networkcontrollerrestore.

 #### <a name="example-restoring-a-network-controller-database"></a>Exemple: La restauration d’une base de données du contrôleur de réseau
 
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

7. Vérifiez la restauration ProvisioningState savoir lorsque la restauration s’est déroulée correctement.

 #### <a name="example-checking-the-status-of-a-network-controller-database-restore"></a>Exemple: Vérification de l’état d’une restauration de la base de données du contrôleur de réseau

    ```
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

8. Si vous utilisez SCVMM, restaurez la base de données à l’aide de la sauvegarde qui a été créée en même temps que la sauvegarde du contrôleur de réseau.

9. Si la charge de travail ordinateurs virtuels est en cours de restauration à partir de la sauvegarde, vous pouvez le faire maintenant.

10. Utilisez l’applet de commande debug-networkcontrollerconfigurationstate pour vérifier l’intégrité de votre système.

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

Pour plus d’informations sur les messages d’état de configuration qui peuvent s’afficher, voir [dépanner le Windows Server2016 défini de mise en réseau pile logicielle](https://docs.microsoft.com/windows-server/networking/sdn/troubleshoot/troubleshoot-windows-server-software-defined-networking-stack).
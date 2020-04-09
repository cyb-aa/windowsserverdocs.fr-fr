---
title: Résoudre les problèmes de la pile de mise en réseau SDN (Software Defined Networking) dans Windows Server
description: Ce guide de Windows Server examine les erreurs SDN (Common Software Defined Networking) et les scénarios d’échec et décrit un flux de travail de dépannage qui tire parti des outils de diagnostic disponibles.
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/14/2018
ms.openlocfilehash: 90e3fd4bde06107871cc3a6b31939ca6b30f2473
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853612"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Résoudre les problèmes de la pile de mise en réseau SDN (Software Defined Networking) dans Windows Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide examine les erreurs et les scénarios d’échec SDN (Common Software Defined Networking) et décrit un flux de travail de dépannage qui tire parti des outils de diagnostic disponibles.  

Pour plus d’informations sur la mise en réseau définie par les logiciels de Microsoft, consultez [mise en réseau à définition logicielle](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Types d’erreurs  
La liste suivante représente la classe des problèmes rencontrés le plus souvent avec la virtualisation de réseau Hyper-V (HNVv1) dans Windows Server 2012 R2 à partir des déploiements de production en cours et coïncide de nombreuses façons avec les mêmes types de problèmes détectés dans Windows Server 2016 HNVv2 avec la nouvelle pile SDN (Software Defined Network).  

La plupart des erreurs peuvent être classées dans un petit ensemble de classes :   
* **Configuration non valide ou non prise en charge**  
   Un utilisateur appelle l’API NorthBound de manière incorrecte ou avec une stratégie non valide.   

* **Erreur dans l’application de stratégie**  
     La stratégie du contrôleur de réseau n’a pas été remise à un hôte Hyper-V, a été considérablement retardée et/ou n’est pas à jour sur tous les ordinateurs hôtes Hyper-V (par exemple, après un Migration dynamique).  
* **Dérive de la configuration ou bogue logiciel**  
  Problèmes liés aux chemins d’accès aux données entraînant la suppression de paquets.  

* **Erreur externe liée au matériel/pilotes de la carte réseau ou à l’infrastructure réseau Underlay**  
  Déchargement des tâches (par exemple, les ordinateurs virtuels) ou l’infrastructure réseau Underlay mal configurée (par exemple, MTU)   

  Ce guide de dépannage examine chacune de ces catégories d’erreur et recommande des méthodes recommandées et des outils de diagnostic disponibles pour identifier et corriger l’erreur.  

## <a name="diagnostic-tools"></a>Outils de diagnostic  

Avant de discuter des flux de travail de dépannage pour chacun de ces types d’erreurs, examinons les outils de diagnostic disponibles.   

Pour utiliser les outils de diagnostic du contrôleur de réseau (contrôle du chemin d’accès), vous devez d’abord installer la fonctionnalité RSAT-NetworkController et importer le module ``NetworkControllerDiagnostics`` :  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Pour utiliser les outils de diagnostic HNV Diagnostics (chemin d’accès aux données), vous devez importer le module ``HNVDiagnostics`` :

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostics du contrôleur de réseau  
Ces applets de commande sont documentées sur TechNet dans la rubrique de l' [applet de commande Diagnostics du contrôleur de réseau](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Ils permettent d’identifier les problèmes de cohérence de la stratégie réseau dans le chemin de contrôle entre les nœuds du contrôleur de réseau et entre le contrôleur de réseau et les agents hôtes NC exécutés sur les ordinateurs hôtes Hyper-V.

 Les applets de commande _Debug-ServiceFabricNodeStatus_ et _NetworkControllerReplica_ doivent être exécutées à partir de l’une des machines virtuelles de nœuds de contrôleur de réseau. Toutes les autres applets de commande de diagnostic NC peuvent être exécutées à partir de n’importe quel ordinateur hôte qui dispose d’une connectivité au contrôleur de réseau et se trouve dans le groupe de sécurité de gestion du contrôleur de réseau (Kerberos) ou a accès au certificat X. 509 pour gérer le contrôleur de réseau. 

### <a name="hyper-v-host-diagnostics"></a>Diagnostics de l’hôte Hyper-V  
Ces applets de commande sont documentées sur TechNet dans la rubrique de l’applet de commande [Diagnostics de la virtualisation de réseau Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Ils permettent d’identifier les problèmes dans le chemin d’accès aux données entre les machines virtuelles du locataire (est/ouest) et le trafic entrant via une adresse IP virtuelle SLB (Nord/Sud). 

Les tests _Debug-VirtualMachineQueueOperation_, _obten-CustomerRoute_, _PACAMapping_, obtient- _ProviderAddress_,- _VMNetworkAdapterPortId_, _obtient-VMSwitchExternalPortId_et _test-EncapOverheadSettings_ sont tous des tests locaux qui peuvent être exécutés à partir de n’importe quel hôte Hyper-V. Les autres applets de commande appellent des tests de chemin d’accès de données via le contrôleur de réseau et, par conséquent, doivent accéder au contrôleur de réseau en tant que descried ci-dessus.

### <a name="github"></a>GitHub
Le [référentiel GitHub Microsoft/SDN](https://github.com/microsoft/sdn) contient un certain nombre d’exemples de scripts et de flux de travail qui s’appuient sur ces applets de commande intégrées. En particulier, les scripts de diagnostic se trouvent dans le dossier [Diagnostics](https://github.com/Microsoft/sdn/diagnostics) . Aidez-nous à contribuer à ces scripts en soumettant des demandes de tirage.

## <a name="troubleshooting-workflows-and-guides"></a>Résolution des problèmes liés aux flux de travail et aux guides  

### <a name="hoster-validate-system-health"></a>Hébergeur Valider l’intégrité du système
Il existe une ressource incorporée appelée état de la _configuration_ dans plusieurs ressources du contrôleur de réseau. L’état de configuration fournit des informations sur l’intégrité du système, notamment la cohérence entre la configuration du contrôleur de réseau et l’état réel (en cours d’exécution) sur les ordinateurs hôtes Hyper-V. 

Pour vérifier l’état de configuration, exécutez la commande suivante à partir de n’importe quel hôte Hyper-V avec connectivité au contrôleur de réseau.

>[!NOTE] 
>La valeur du paramètre *NetworkController* doit être le nom de domaine complet ou l’adresse IP en fonction du nom du sujet du certificat de > X. 509 créé pour le contrôleur de réseau.
>
>Le paramètre *Credential* doit être spécifié uniquement si le contrôleur de réseau utilise l’authentification Kerberos (typique dans les déploiements VMM). Les informations d’identification doivent être pour un utilisateur qui se trouve dans le groupe de sécurité de gestion du contrôleur de réseau.

```none
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

Un exemple de message d’état de configuration est présenté ci-dessous :

```none
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> Il existe un bogue dans le système où les ressources de l’interface réseau pour la carte réseau de l’ordinateur virtuel en transit MUX SLB sont dans un état d’échec avec l’erreur « commutateur virtuel-hôte non connecté au contrôleur ». Cette erreur peut être ignorée en toute sécurité si la configuration IP de la ressource de carte réseau de la machine virtuelle est définie sur une adresse IP du pool d’adresses IP du réseau logique de transit. Il y a un deuxième bogue dans le système où les ressources de l’interface réseau pour les cartes réseau de l’ordinateur virtuel du fournisseur HNV de la passerelle sont dans un état d’échec avec l’erreur « virtual switch-PortBlocked ». Cette erreur peut également être ignorée en toute sécurité si la configuration IP de la ressource de carte réseau de la machine virtuelle est définie sur null (par conception).


Le tableau ci-dessous présente la liste des codes d’erreur, des messages et des actions de suivi à entreprendre en fonction de l’état de configuration observé.


| **Code**| **Message**| **Action**|  
|--------|-----------|----------|  
| Inconnu.| Erreur inconnue| |  
| HostUnreachable                       | L’ordinateur hôte n’est pas accessible | Vérifier la connectivité réseau de gestion entre le contrôleur de réseau et l’hôte |  
| PAIpAddressExhausted                  | Les adresses IP PA sont épuisées | Augmenter la taille du pool d’adresses IP du sous-réseau logique du fournisseur HNV |  
| PAMacAddressExhausted                 | Les adresses Mac PA sont épuisées | Augmenter la plage du pool d’adresses Mac |  
| PAAddressConfigurationFailure         | Échec du raccordement des adresses PA à l’hôte | Vérifiez la connectivité réseau de gestion entre le contrôleur de réseau et l’hôte. |  
| CertificateNotTrusted                 | Le certificat n’est pas approuvé  |Corrigez les certificats utilisés pour la communication avec l’hôte. |  
| CertificateNotAuthorized              | Certificat non autorisé | Corrigez les certificats utilisés pour la communication avec l’hôte. |  
| PolicyConfigurationFailureOnVfp       | Échec de la configuration des stratégies VFP | Il s’agit d’un échec d’exécution.  Aucune solution de contournement n’est définie. Collecte des journaux. |  
| PolicyConfigurationFailure            | Échec de l’envoi des stratégies aux hôtes, en raison d’échecs de communication ou d’autres erreurs dans le NetworkController.| Aucune action définie.  Cela est dû à une défaillance du traitement de l’état de l’objectif dans les modules de contrôleur de réseau. Collecte des journaux. |  
| HostNotConnectedToController          | L’ordinateur hôte n’est pas encore connecté au contrôleur de réseau. | Le profil de port n’est pas appliqué sur l’ordinateur hôte ou l’ordinateur hôte n’est pas accessible à partir du contrôleur de réseau. Vérifiez que la clé de Registre HostID correspond à l’ID d’instance de la ressource de serveur |  
| MultipleVfpEnabledSwitches            | Plusieurs commutateurs VFp sont activés sur l’hôte  | Supprimez l’un des commutateurs, car l’agent hôte du contrôleur de réseau ne prend en charge qu’un seul vSwitch avec l’extension VFP activée |  
| PolicyConfigurationFailure            | Échec de l’envoi des stratégies de réseau virtuel pour un carte réseau en raison d’erreurs de certificat ou de connectivité  | Vérifiez si des certificats appropriés ont été déployés (le nom d’objet du certificat doit correspondre au nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau. |  
| PolicyConfigurationFailure            | Échec de l’émission des stratégies vSwitch pour un carte réseau en raison d’erreurs de certificat ou d’erreurs de connectivité  | Vérifiez si des certificats appropriés ont été déployés (le nom d’objet du certificat doit correspondre au nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau.|
| PolicyConfigurationFailure            | Échec de l’envoi de stratégies de pare-feu pour un carte réseau en raison d’erreurs de certificat ou de connectivité | Vérifiez si des certificats appropriés ont été déployés (le nom d’objet du certificat doit correspondre au nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau.|
| DistributedRouterConfigurationFailure | Échec de la configuration des paramètres du routeur distribué sur l’ordinateur hôte carte réseau virtuelle                          | Erreur de pile TCPIP. Peut nécessiter le nettoyage des cartes réseau virtuelles d’hôte d’ordinateur hôte et de récupération d’urgence sur le serveur sur lequel cette erreur a été signalée |
| DhcpAddressAllocationFailure          | Échec de l’allocation d’adresses DHCP pour un carte réseau                                                    | Vérifier si l’attribut d’adresse IP statique est configuré sur la ressource de la carte réseau |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Échec de la connexion au multiplexeur en raison d’erreurs réseau ou de certificat | Vérifiez le code numérique fourni dans le code du message d’erreur : cela correspond au code d’erreur Winsock. Les erreurs de certificat sont granulaires (par exemple, le certificat ne peut pas être vérifié, le certificat n’est pas autorisé, etc.) |  
| HostUnreachable                       | MUX non intègre (le cas courant est BGPRouter déconnecté) | L’homologue BGP sur le commutateur RRAS (machine virtuelle BGP) ou le commutateur Top-of-rack (TDR) est inaccessible ou ne fonctionne pas correctement. Vérifier les paramètres BGP sur la ressource logicielle Load Balancer multiplexer et l’homologue BGP (TDR ou machine virtuelle RRAS) |  
| HostNotConnectedToController          | L’agent hôte SLB n’est pas connecté  | Vérifiez que le service de l’agent hôte SLB est en cours d’exécution ; Reportez-vous aux journaux de l’agent hôte SLB (exécution automatique) pour savoir pourquoi, si SLBM (NC) a rejeté le certificat présenté par l’État en cours d’exécution de l’agent hôte, affichera les informations en nuances  |  
| PortBlocked                           | Le port VFP est bloqué, en raison d’un manque de stratégies de réseau virtuel/ACL | Vérifiez s’il existe d’autres erreurs, ce qui peut empêcher la configuration des stratégies. |  
| Surchargé                            | LoadBalancer MUX est surchargé  | Problème de performances avec MUX |  
| RoutePublicationFailure               | LoadBalancer MUX n’est pas connecté à un routeur BGP | Vérifiez si le MULTIPLEXeur dispose d’une connectivité avec les routeurs BGP et que l’homologation BGP est correctement configurée |  
| VirtualServerUnreachable              | LoadBalancer MUX n’est pas connecté au gestionnaire SLB | Vérifier la connectivité entre SLBM et MUX |  
| QosConfigurationFailure               | Échec de la configuration des stratégies QOS | Vérifiez si une bande passante suffisante est disponible pour toutes les machines virtuelles si la réservation QOS est utilisée |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Vérifier la connectivité réseau entre le contrôleur de réseau et l’hôte Hyper-V (service de l’agent hôte NC)
Exécutez la commande *netstat* ci-dessous pour vérifier qu’il existe trois connexions établies entre l’agent hôte NC et le ou les nœuds du contrôleur de réseau et un socket d’écoute sur l’hôte Hyper-V.
- ÉCOUTE sur le port TCP : 6640 sur l’hôte Hyper-V (service de l’agent hôte NC)
- Deux connexions établies à partir de l’adresse IP de l’hôte Hyper-V sur le port 6640 à l’adresse IP du nœud NC sur les ports éphémères (> 32000)
- Une connexion établie entre l’adresse IP de l’hôte Hyper-V sur le port éphémère et l’adresse IP REST du contrôleur de réseau sur le port 6640

>[!NOTE]
>Il se peut qu’il n’y ait que deux connexions établies sur un hôte Hyper-V s’il n’y a pas d’ordinateurs virtuels locataires déployés sur cet ordinateur hôte particulier.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Vérifier les services de l’agent hôte
Le contrôleur de réseau communique avec deux services de l’agent hôte sur les ordinateurs hôtes Hyper-V : agent hôte SLB et agent hôte NC. Il est possible qu’au moins l’un de ces services ne soit pas en cours d’exécution. Vérifiez leur état et redémarrez-les s’ils ne sont pas en cours d’exécution.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Vérifier l’intégrité du contrôleur de réseau
S’il n’y a pas trois connexions établies ou si le contrôleur de réseau semble ne pas répondre, vérifiez que tous les nœuds et les modules de service sont en cours d’exécution à l’aide des applets de commande suivantes. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Les modules de service de contrôleur de réseau sont les suivants :
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService

Vérifiez que ReplicaStatus est **prêt** et que HealthState est **OK**.

Dans un déploiement de production avec un contrôleur de réseau à plusieurs nœuds, vous pouvez également vérifier le nœud principal de chaque service et son état de réplica individuel.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Vérifiez que l’état du réplica est prêt pour chaque service.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Rechercher les HostIDs et certificats correspondants entre le contrôleur de réseau et chaque hôte Hyper-V 
Sur un ordinateur hôte Hyper-V, exécutez les commandes suivantes pour vérifier que HostID correspond à l’ID d’instance d’une ressource de serveur sur le contrôleur de réseau.

```none
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

Mise à *jour* Si vous utilisez des scripts SDNExpress ou un déploiement manuel, mettez à jour la clé HostId dans le registre pour qu’elle corresponde à l’ID d’instance de la ressource de serveur. Redémarrez l’agent hôte du contrôleur de réseau sur l’hôte Hyper-V (serveur physique) si vous utilisez VMM, supprimez le serveur Hyper-V de VMM et supprimez la clé de Registre HostId. Ensuite, ajoutez de nouveau le serveur via VMM.


Vérifiez que les empreintes numériques des certificats X. 509 utilisés par l’hôte Hyper-V (le nom d’hôte est le nom du sujet du certificat) pour la communication (SouthBound) entre l’hôte Hyper-V (service agent hôte NC) et les nœuds du contrôleur de réseau sont identiques. Vérifiez également que le certificat REST du contrôleur de réseau a le nom d’objet *CN =<FQDN or IP>* .

```  
# On Hyper-V Host
dir cert:\\localmachine\my  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```  

Vous pouvez également vérifier les paramètres suivants de chaque certificat pour vous assurer que le nom de l’objet correspond à ce qui est attendu (nom d’hôte ou nom de domaine complet (FQDN) REST ou IP), que le certificat n’a pas encore expiré et que toutes les autorités de certification de la chaîne de certificats sont incluses dans la racine approuvée. autorisations.

- Nom d'objet  
- Date d'expiration  
- Approuvé par l’autorité racine  

Mise à *jour* Si plusieurs certificats ont le même nom d’objet sur l’hôte Hyper-V, l’agent hôte du contrôleur de réseau en choisit un au hasard dans le contrôleur de réseau. Cela peut ne pas correspondre à l’empreinte numérique de la ressource de serveur connue du contrôleur de réseau. Dans ce cas, supprimez l’un des certificats portant le même nom d’objet sur l’hôte Hyper-V, puis redémarrez le service agent hôte du contrôleur de réseau. Si une connexion ne peut toujours pas être établie, supprimez l’autre certificat portant le même nom d’objet sur l’hôte Hyper-V et supprimez la ressource serveur correspondante dans VMM. Ensuite, recréez la ressource de serveur dans VMM, ce qui génère un nouveau certificat X. 509 et l’installe sur l’hôte Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Vérifier l’état de la configuration SLB
L’état de configuration SLB peut être déterminé dans le cadre de la sortie de l’applet de commande Debug-NetworkController. Cette applet de commande génère également le jeu actuel de ressources du contrôleur de réseau dans les fichiers JSON, toutes les configurations IP de chaque hôte Hyper-V (serveur) et la stratégie de réseau local des tables de la base de données de l’agent hôte. 

Les suivis supplémentaires seront collectés par défaut. Pour ne pas collecter les traces, ajoutez le paramètre-IncludeTraces : $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>L’emplacement de sortie par défaut est le < working_directory > Répertoire \NCDiagnostics\. Le répertoire de sortie par défaut peut être modifié à l’aide du paramètre `-OutputDirectory`. 

Les informations sur l’état de configuration SLB se trouvent dans le fichier _Diagnostics-slbstateResults. JSON_ dans ce répertoire.

Ce fichier JSON peut être divisé en sections suivantes :
 * Infrastructure
   * SlbmVips : cette section répertorie l’adresse IP de l’adresse IP virtuelle du gestionnaire SLB qui est utilisée par le contrôleur de réseau pour coodinate la configuration et l’intégrité entre les agents hôtes SLB multiplexeurs et SLB.
   * MuxState : cette section répertorie une valeur pour chaque multiplexeur SLB déployé, donnant l’état du multiplexeur
   * Configuration du routeur : cette section répertorie le numéro de système autonome (ASN) du routeur en amont, l’adresse IP de transit et l’ID. Elle répertorie également l’adresse IP de transit et le ASN multiplexeurs SLB.
   * Informations sur l’hôte connecté : cette section répertorie l’adresse IP de gestion de tous les ordinateurs hôtes Hyper-V disponibles pour exécuter des charges de travail à charge équilibrée.
   * Plages d’adresses IP virtuelles : cette section répertorie les plages de pools d’adresses IP VIRTUELles publiques et privées. L’adresse IP virtuelle SLBM sera incluse en tant qu’adresse IP allouée à partir de l’une de ces plages. 
   * Itinéraires Mux : cette section répertorie une valeur pour chaque multiplexeur SLB déployé contenant toutes les publications d’itinéraires pour ce multiplexeur particulier.
 * Locataire
   * VipConsolidatedState : cette section répertorie l’état de la connectivité pour chaque adresse IP virtuelle de locataire, y compris le préfixe d’itinéraire publié, l’hôte Hyper-V et les points de terminaison DIP.

> [!NOTE]
> L’État SLB peut être déterminé directement à l’aide du script [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) disponible dans le [référentiel GitHub Microsoft SDN](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validation de la passerelle

**À partir du contrôleur de réseau :**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**À partir de la machine virtuelle de passerelle :**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**À partir du commutateur de haut en rack (TDR) :**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Routeur BGP Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

En plus de ceux-ci, des problèmes que nous avons constatés jusqu’à présent (en particulier sur les déploiements basés sur SDNExpress), la raison la plus courante du compartiment de locataire non configuré sur les machines virtuelles GW semble être le fait que la capacité GW dans FabricConfig. psd1 est moins comparée à ce les gens essaient d’attribuer des connexions réseau (Tunnels S2S) dans TenantConfig. psd1. Vous pouvez le vérifier facilement en comparant les sorties des commandes suivantes :

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>Hébergeur Valider le plan de données
Une fois le contrôleur de réseau déployé, les réseaux virtuels et les sous-réseaux locataires ont été créés, et les machines virtuelles ont été attachées aux sous-réseaux virtuels, les tests de niveau Fabric supplémentaires peuvent être effectués par l’hébergeur pour vérifier la connectivité du locataire.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Vérifier la connectivité réseau logique du fournisseur HNV
Une fois que la première machine virtuelle invitée exécutée sur un ordinateur hôte Hyper-V a été connectée à un réseau virtuel locataire, le contrôleur de réseau affecte deux adresses IP de fournisseur HNV (adresses IP PA) à l’hôte Hyper-V. Ces adresses IP sont issues du pool d’adresses IP du réseau logique du fournisseur HNV et sont gérées par le contrôleur de réseau.  Pour savoir ce que sont les deux adresses IP HNV

```none
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

Ces adresses IP de fournisseur HNV (IPs PA) sont affectées aux cartes Ethernet créées dans un compartiment réseau TCP/IP distinct et ont le nom d’adaptateur _VLANX_ où X est le réseau local virtuel affecté au réseau logique du fournisseur HNV (transport).

La connectivité entre deux hôtes Hyper-V à l’aide du réseau logique du fournisseur HNV peut être effectuée par un test ping avec un autre paramètre de compartiment (-c Y) où Y est le compartiment réseau TCPIP dans lequel les PAhostVNICs sont créés. Ce compartiment peut être déterminé en exécutant :

```none
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> Les adaptateurs carte réseau virtuelle de l’hôte PA ne sont pas utilisés dans le chemin de données et n’ont donc pas d’adresse IP affectée à l’adaptateur « vEthernet (PAhostVNic) ».

Par exemple, supposons que les hôtes Hyper-V 1 et 2 ont des adresses IP du fournisseur HNV (PA) :

|-Hôte Hyper-V-|-Adresse IP PA 1|-Adresse IP PA 2|
|---             |---            |---             |
|Hôte 1 | 10.10.182.64 | 10.10.182.65 |
|Hôte 2 | 10.10.182.66 | 10.10.182.67 |

Nous pouvons effectuer un test ping entre les deux à l’aide de la commande suivante pour vérifier la connectivité réseau logique du fournisseur HNV.

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

Mise à *jour* Si le test ping du fournisseur HNV ne fonctionne pas, vérifiez votre connectivité réseau physique, y compris la configuration du réseau local virtuel. Les cartes réseau physiques sur chaque hôte Hyper-V doivent être en mode Trunk sans réseau local virtuel spécifique. Le carte réseau virtuelle de l’hôte de gestion doit être isolé sur le VLAN du réseau logique de gestion.

```none
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...        
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...
```

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Vérifier la prise en charge des trames MTU et Jumbo sur le réseau logique du fournisseur HNV

Un autre problème courant dans le réseau logique de fournisseur HNV est que les ports réseau physiques et/ou la carte Ethernet ne disposent pas d’un MTU suffisamment important configuré pour gérer la surcharge à partir de l’encapsulation VXLAN (ou NVGRE). 
>[!NOTE]
> Certaines cartes et Pilotes Ethernet prennent en charge le nouveau mot clé * EncapOverhead qui est automatiquement défini par l’agent hôte du contrôleur de réseau sur la valeur 160. Cette valeur sera ensuite ajoutée à la valeur du mot clé * JumboPacket dont la somme est utilisée comme MTU publiée.
> par exemple, * EncapOverhead = 160 et * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Pour déterminer si le réseau logique du fournisseur HNV prend ou non en charge la plus grande taille MTU de bout en bout, utilisez l’applet de commande _test-LogicalNetworkSupportsJumboPacket_ :
```none
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.


```

*Correction*
* Ajustez la taille MTU sur les ports de commutateur physique pour qu’elle soit au moins 1674B (y compris l’en-tête Ethernet 14 ter et le code de fin)
* Si votre carte réseau ne prend pas en charge le mot clé EncapOverhead, ajustez le mot clé JumboPacket sur au moins 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Vérifier la connectivité des cartes réseau d’ordinateur virtuel client
Chaque carte réseau de machine virtuelle affectée à une machine virtuelle invitée a un mappage CA-PA entre l’adresse client privée (CA) et l’espace d’adresse du fournisseur HNV (PA). Ces mappages sont conservés dans les tables du serveur OVSDB sur chaque hôte Hyper-V et sont accessibles en exécutant l’applet de commande suivante.

```none
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66
```
>[!NOTE]
> Si les mappages CA-PA que vous attendez ne sont pas générés pour une machine virtuelle cliente donnée, vérifiez les ressources de la carte réseau de la machine virtuelle et de la configuration IP sur le contrôleur de réseau à l’aide de l’applet de commande _NetworkControllerNetworkInterface_ . Vérifiez également les connexions établies entre l’agent hôte NC et les nœuds du contrôleur de réseau.

Avec ces informations, un test ping de machine virtuelle client peut désormais être initié par l’hébergeur à partir du contrôleur de réseau à l’aide de l’applet de commande _test-VirtualNetworkConnection_ .

## <a name="specific-troubleshooting-scenarios"></a>Scénarios de résolution des problèmes spécifiques

Les sections suivantes fournissent des conseils pour résoudre des scénarios spécifiques.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Aucune connectivité réseau entre deux machines virtuelles clientes

1.  Priorité Assurez-vous que le pare-feu Windows dans les ordinateurs virtuels locataires ne bloque pas le trafic.  
2.  Priorité Vérifiez que les adresses IP ont été affectées à la machine virtuelle du locataire en exécutant _ipconfig_. 
3.  Hébergeur Exécutez **test-VirtualNetworkConnection** à partir de l’hôte Hyper-V pour valider la connectivité entre les deux machines virtuelles clientes en question. 

>[!NOTE]
>La valeur de l’ID de la sécurité correspond à l’ID de sous-réseau virtuel. Dans le cas de VXLAN, il s’agit de l’identificateur de réseau VXLAN (VNI). Vous pouvez trouver cette valeur en exécutant l’applet de commande **PACAMapping** .

#### <a name="example"></a>Exemple

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Créer une autorité de certification-ping entre la « machine virtuelle Green Web 1 » et l’adresse IP SenderCA 192.168.1.4 sur l’ordinateur hôte « sa18n30-2.sa18.nttest.microsoft.com » avec l’adresse IP de gestion 10.127.132.153 à l’adresse IP ListenerCA des 192.168.1.5 attachés au sous-réseau virtuel (\ n) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Démarrage du test ping de l’espace de l’autorité de certification démarrage du test ping de session sur 192.168.1.5 réussie à partir de l’adresse 192.168.1.4 RTT = 0 ms


Informations de routage de l’autorité de certification :

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informations de routage PA :

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip>...

4. Priorité Vérifiez qu’aucune stratégie de pare-feu distribuée n’est spécifiée sur le sous-réseau virtuel ou les interfaces réseau de machine virtuelle qui bloquent le trafic.    

Interrogez l’API REST du contrôleur de réseau qui se trouve dans l’environnement de démonstration sur sa18n30nc dans le domaine sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

## <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examiner la configuration IP et les sous-réseaux virtuels qui référencent cette liste de contrôle d’accès

1. Hébergeur Exécutez ``Get-ProviderAddress`` sur les hôtes Hyper-V hébergeant les deux machines virtuelles clientes en question, puis exécutez ``Test-LogicalNetworkConnection`` ou ``ping -c <compartment>`` à partir de l’hôte Hyper-V pour valider la connectivité sur le réseau logique du fournisseur HNV
2.  Hébergeur Assurez-vous que les paramètres MTU sont corrects sur les hôtes Hyper-V et les périphériques de commutation de couche 2 entre les hôtes Hyper-V. Exécutez ``Test-EncapOverheadValue`` sur tous les ordinateurs hôtes Hyper-V en question. Vérifiez également que tous les commutateurs de couche 2 entre ont une MTU définie sur un minimum de 1674 octets pour prendre en compte une charge maximale de 160 octets.  
3.  Hébergeur Si les adresses IP PA ne sont pas présentes et/ou si la connectivité de l’autorité de certification est interrompue, vérifiez que la stratégie réseau a été reçue. Exécutez ``Get-PACAMapping`` pour voir si les règles d’encapsulation et les mappages CA-PA requis pour la création de réseaux virtuels de superposition sont correctement établis.  
4.  Hébergeur Vérifiez que l’agent hôte du contrôleur de réseau est connecté au contrôleur de réseau. Exécutez ``netstat -anp tcp |findstr 6640`` pour voir si le   
5.  Hébergeur Vérifiez que l’ID d’hôte dans HKLM/correspond à l’ID d’instance des ressources serveur qui hébergent les machines virtuelles clientes.  
6. Hébergeur Vérifiez que l’ID de profil de port correspond à l’ID d’instance des interfaces réseau de machine virtuelle des machines virtuelles clientes.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Journalisation, suivi et diagnostics avancés

Les sections suivantes fournissent des informations sur les diagnostics avancés, la journalisation et le suivi.

### <a name="network-controller-centralized-logging"></a>Journalisation centralisée du contrôleur de réseau 

Le contrôleur de réseau peut collecter automatiquement les journaux du débogueur et les stocker dans un emplacement centralisé. La collecte des journaux peut être activée lorsque vous déployez le contrôleur de réseau pour la première fois ou à tout moment ultérieurement. Les journaux sont collectés à partir du contrôleur de réseau, et les éléments réseau gérés par le contrôleur de réseau : ordinateurs hôtes, équilibreurs de charge logicielle (SLB) et ordinateurs passerelle. 

Ces journaux incluent les journaux de débogage pour le cluster de contrôleur de réseau, l’application de contrôleur de réseau, les journaux de passerelle, SLB, le réseau virtuel et le pare-feu distribué. Chaque fois qu’un nouvel hôte/SLB/passerelle est ajouté au contrôleur de réseau, la journalisation est démarrée sur ces ordinateurs. De même, lorsqu’un ordinateur hôte/SLB/passerelle est supprimé du contrôleur de réseau, la journalisation est arrêtée sur ces machines.

#### <a name="enable-logging"></a>Activer la journalisation

La journalisation est activée automatiquement lorsque vous installez le cluster de contrôleur de réseau à l’aide de l’applet **de commande Install-NetworkControllerCluster** . Par défaut, les journaux sont collectés localement sur les nœuds du contrôleur de réseau sur *%systemdrive%\SDNDiagnostics*. Il est **fortement recommandé** de modifier cet emplacement pour qu’il soit un partage de fichiers distant (non local). 

Les journaux de cluster du contrôleur de réseau sont stockés dans *%ProgramData%\Windows Fabric\log\Traces*. Vous pouvez spécifier un emplacement centralisé pour la collecte de journaux avec le paramètre **DiagnosticLogLocation** avec la recommandation qu’il s’agit également d’un partage de fichiers distant. 

Si vous souhaitez restreindre l’accès à cet emplacement, vous pouvez fournir les informations d’identification d’accès avec le paramètre **LogLocationCredential** . Si vous fournissez les informations d’identification pour accéder à l’emplacement du journal, vous devez également fournir le paramètre **CredentialEncryptionCertificate** , qui est utilisé pour chiffrer les informations d’identification stockées localement sur les nœuds du contrôleur de réseau.  

Avec les paramètres par défaut, il est recommandé de disposer d’au moins 75 Go d’espace libre dans l’emplacement central et de 25 Go sur les nœuds locaux (si vous n’utilisez pas un emplacement central) pour un cluster de contrôleur de réseau à 3 nœuds.

#### <a name="change-logging-settings"></a>Modifier les paramètres de journalisation

Vous pouvez modifier les paramètres de journalisation à tout moment à l’aide de l’applet de commande ``Set-NetworkControllerDiagnostic``. Les paramètres suivants peuvent être modifiés :

- **Emplacement centralisé des journaux**.  Vous pouvez modifier l’emplacement de stockage de tous les journaux, avec le paramètre ``DiagnosticLogLocation``.  
- **Informations d’identification pour accéder à l’emplacement du journal**.  Vous pouvez modifier les informations d’identification pour accéder à l’emplacement du journal, avec le paramètre ``LogLocationCredential``.  
- **Accédez à journalisation locale**.  Si vous avez fourni un emplacement centralisé pour stocker les journaux, vous pouvez revenir à la journalisation locale sur les nœuds du contrôleur de réseau avec le paramètre ``UseLocalLogLocation`` (non recommandé en raison de l’espace disque nécessaire).  
- **Étendue de journalisation**.  Par défaut, tous les journaux sont collectés. Vous pouvez modifier l’étendue pour collecter uniquement les journaux de cluster de contrôleur de réseau.  
- **Niveau de journalisation**.  Le niveau de journalisation par défaut est informatif. Vous pouvez le remplacer par Error, Warning ou Verbose.  
- **Heure**de la datation des journaux.  Les journaux sont stockés de manière circulaire. Vous aurez 3 jours de données de journalisation par défaut, que vous utilisiez la journalisation locale ou la journalisation centralisée. Vous pouvez modifier cette limite de temps avec le paramètre **LogTimeLimitInDays** .  
- **Taille**de la datation des journaux.  Par défaut, vous aurez un maximum de 75 Go de données de journalisation si vous utilisez la journalisation centralisée et 25 Go si vous utilisez la journalisation locale. Vous pouvez modifier cette limite à l’aide du paramètre **LogSizeLimitInMBs** .

#### <a name="collecting-logs-and-traces"></a>Collecte des journaux et des suivis

Les déploiements VMM utilisent la journalisation centralisée pour le contrôleur de réseau par défaut. L’emplacement du partage de fichiers pour ces journaux est spécifié lors du déploiement du modèle de service de contrôleur de réseau.

Si aucun emplacement de fichier n’a été spécifié, la journalisation locale est utilisée sur chaque nœud de contrôleur de réseau avec les journaux enregistrés sous C:\Windows\tracing\SDNDiagnostics. Ces journaux sont enregistrés à l’aide de la hiérarchie suivante :

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Traces

Le contrôleur de réseau utilise (Azure) Service Fabric. Les journaux de Service Fabric peuvent être nécessaires pour résoudre certains problèmes. Ces journaux se trouvent sur chaque nœud de contrôleur de réseau dans C:\ProgramData\Microsoft\Service fabric.

Si un utilisateur a exécuté l’applet de commande _Debug-NetworkController_ , des journaux supplémentaires sont disponibles sur chaque hôte Hyper-V qui a été spécifié avec une ressource de serveur dans le contrôleur de réseau. Ces journaux (et les suivis s’ils sont activés) sont conservés sous C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostics SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erreurs d’infrastructure SLBM (actions du fournisseur de services d’hébergement)

1.  Vérifiez que le gestionnaire de Load Balancer logiciel (SLBM) fonctionne et que les couches d’orchestration peuvent communiquer entre elles : agents hôtes SLB > SLB MUX et SLBM-> SLB. Exécutez [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) à partir de n’importe quel nœud ayant accès au point de terminaison REST du contrôleur de réseau.  
2.  Validez le *SDNSLBMPerfCounters* dans Perfmon sur l’une des machines virtuelles de nœud de contrôleur de réseau (de préférence le contrôleur de réseau principal node-to-NetworkControllerReplica) :
    1.  Le moteur Load Balancer (LB) est-il connecté à SLBM ? (*LBEngines de configuration* de la configuration de l’ensemble des configurations SLBM > 0)  
    2.  SLBM a-t-il au moins connaissance de ses propres points de terminaison ? (*Nombre total de points de terminaison VIP* > = 2)  
    3.  Les hôtes Hyper-V (DIP) sont-ils connectés à SLBM ? (*Clients HP connectés* = = Nbre de serveurs)   
    4.  SLBM est-il connecté à multiplexeurs ? (*Multiplexeurs connecté* == *multiplexeurs sain sur SLBM* == *multiplexeurs Reporting sain* = # SLB multiplexeurs machines virtuelles).  
3.  Vérifiez que le routeur BGP configuré s’apparie correctement au MULTIPLEXeur SLB.  
    1.  Si vous utilisez RRAS avec l’accès à distance (par exemple, une machine virtuelle BGP) :  
        1.  L’BgpPeer doit afficher la connexion  
        2.  La méthode BgpRouteInformation doit afficher au moins un itinéraire pour l’auto-VIP SLBM  
    2.  Si vous utilisez le commutateur physique supérieur à rack (TDR) comme homologue BGP, consultez votre documentation  
        1.  Par exemple : # Show BGP instance  
4.  Valider les compteurs *SlbMuxPerfCounters* et *SLBMUX* dans Perfmon sur la machine virtuelle SLB MUX
5.  Vérifier l’état de configuration et les plages d’adresses IP virtuelles dans la ressource Software Load Balancer Manager  
    1.  Obtient-NetworkControllerLoadBalancerConfiguration-ConnectionUri < https://<FQDN or IP>| ConvertTo-JSON-Depth 8 (Vérifiez les plages d’adresses IP virtuelles dans les pools d’adresses IP et assurez-vous que SLBM Self-VIP (*LoadBalanacerManagerIPAddress*) et toutes les adresses IP virtuelles côté locataire se trouvent dans ces plages)  
        1. Obtient-NetworkControllerIpPool-NetworkId « < ID de ressource de réseau logique d’adresses IP virtuelles publiques/privées > «-SubnetId » < ID de ressource de sous-réseau logique VIP publique/privée > « -ResourceId »<IP Pool Resource Id>»-ConnectionUri $uri | ConvertTo-JSON-Depth 8 
    2.  Débogage-NetworkControllerConfigurationState-  

Si l’une des vérifications ci-dessus échoue, l’État SLB du locataire est également en mode échec.  

  de **Correction**  
En fonction des informations de diagnostic présentées ci-dessous, corrigez les éléments suivants :  
* Garantir la connexion des multiplexeurs SLB  
  * Résoudre les problèmes de certificat  
  * Résoudre les problèmes de connectivité réseau  
* Vérifier que les informations d’appairage BGP sont correctement configurées  
* Vérifiez que l’ID d’hôte dans le registre correspond à l’ID d’instance de serveur dans la ressource de serveur (annexe de référence pour le code d’erreur *HostNotConnected* )  
* Collecter les journaux  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erreurs de locataires SLBM (fournisseur de services d’hébergement et actions de locataire)

1.  Hébergeur Cochez *Debug-NetworkControllerConfigurationState* pour voir si des ressources LoadBalancer sont dans un état d’erreur. Essayez d’atténuer en suivant le tableau des éléments d’action dans l’annexe.   
    1.  Vérifier la présence d’un point de terminaison VIP et des itinéraires de publication  
    2.  Vérifier le nombre de points de terminaison DIP détectés pour le point de terminaison VIP  
2.  Priorité Valider les ressources Load Balancer correctement spécifiées  
    1.  Valider les points de terminaison DIP inscrits dans SLBM qui hébergent des machines virtuelles clientes qui correspondent aux configurations IP du pool d’adresses principales de LoadBalancer  
3.  Hébergeur Si les points de terminaison DIP ne sont pas découverts ou connectés :   
    1.  Vérifier *Debug-NetworkControllerConfigurationState*  
        1.  Vérifiez que l’agent hôte NC et SLB est correctement connecté au coordinateur d’événements du contrôleur de réseau à l’aide de ``netstat -anp tcp |findstr 6640)``  
    2.  Vérifiez l’ID d’instance de la ressource de serveur correspondante (``Get-NCServer |convertto-json -depth 8``) dans la case à cocher *HostID* dans la RegKey du service *nchostagent* (référence du code d’erreur *HostNotConnected* de l’annexe)  
    3.  Vérifier l’ID de profil de port pour le port de l’ordinateur virtuel correspond à l’ID d’instance de la ressource de carte réseau de l’ordinateur virtuel   
4.  [Fournisseur d’hébergement] Collecter les journaux   

#### <a name="slb-mux-tracing"></a>Suivi SLB MUX

Les informations du logiciel Load Balancer multiplexeurs peuvent également être déterminées via observateur d’événements. 
1. Cliquez sur Afficher les journaux d’analyse et de débogage dans le menu Affichage de observateur d’événements
2. Accédez à « journaux des applications et des services » > Microsoft > Windows > SlbMuxDriver > trace dans observateur d’événements 
3. Cliquez avec le bouton droit sur celui-ci et sélectionnez « Activer le journal ».

>[!NOTE]
>Il est recommandé de ne pas activer cette journalisation pendant une brève période pendant que vous essayez de reproduire un problème

### <a name="vfp-and-vswitch-tracing"></a>Suivi VFP et vSwitch

À partir de n’importe quel ordinateur hôte Hyper-V qui héberge une machine virtuelle invitée attachée à un réseau virtuel locataire, vous pouvez collecter une trace VFP pour déterminer où les problèmes peuvent se trouver.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```

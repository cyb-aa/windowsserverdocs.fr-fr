---
title: Résoudre les problèmes de la pile de mise en réseau Windows Server logiciel défini
description: Ce guide de Windows Server analyse les erreurs courantes de logiciel défini de mise en réseau (SDN) et les scénarios d’échec et présente un flux de travail de résolution des problèmes qui exploite les outils de diagnostic disponibles.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Résoudre les problèmes de la pile de mise en réseau Windows Server logiciel défini

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Ce guide examine les erreurs courantes de logiciel défini de mise en réseau (SDN) et les scénarios d’échec et présente un flux de travail de résolution des problèmes qui exploite les outils de diagnostic disponibles.  

Pour plus d’informations sur logiciels définies mise en réseau de Microsoft, voir [logiciel défini de mise en réseau](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Types d’erreurs  
La liste suivante représente la classe de problèmes le plus souvent avec la virtualisation de réseau Hyper-V (HNVv1) dans Windows Server2012R2 à partir des déploiements de production dans le marché et coïncide de nombreuses manières avec les mêmes types de problèmes dans Windows Server2016 HNVv2 avec la nouvelle pile logicielle définie réseau (SDN).  

La plupart des erreurs peuvent être classés dans un petit ensemble de classes:   
* **Configuration non valide ou non pris en charge**  
   Un utilisateur appelle l’API NorthBound ou de façon incorrecte avec la stratégie non valide.   

* **Erreur dans l’application de la stratégie**  
     Stratégie à partir du contrôleur de réseau n’a pas été remis à un ordinateur hôte Hyper-V, et / considérablement retardé ou non mis à jour sur tous les hôtes Hyper-V (par exemple, après une Migration dynamique).  
* **Bogue dérive ou logiciels de configuration**  
 Problèmes de chemin d’accès de données résultant dans les paquets ignorés.  

* **Erreur externe lié au matériel de carte réseau / pilotes ou sous-couche de l’infrastructure de réseau**  
 Mauvais déchargements de tâche (par exemple, VMQ) ou infrastructure de réseau de sous-couche mal configurée (par exemple, MTU)   

 Ce guide de dépannage examine chacune de ces catégories d’erreur et recommande des meilleures pratiques et les outils de diagnostic disponibles pour identifier et résoudre l’erreur.  

## <a name="diagnostic-tools"></a>Outils de diagnostic  

Avant d’aborder les flux de travail de résolution des problèmes pour chacun de ces types d’erreurs, nous allons examiner les outils de diagnostic.   
  
Pour utiliser les outils de diagnostic de contrôleur de réseau (contrôle de chemin d’accès), vous devez tout d’abord installer la fonctionnalité de RSAT-NetworkController et importer la ``NetworkControllerDiagnostics``module:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Pour utiliser les outils de diagnostic HNV Diagnostics (chemin de données), vous devez importer le ``HNVDiagnostics``module:
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostic de contrôleur de réseau  
Ces applets de commande sont documentées sur TechNet dans le [réseau contrôleur Diagnostics applet de commande rubrique](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/). Ils permettent d’identifier les problèmes de cohérence de stratégie de réseau dans le chemin de contrôle entre les nœuds de contrôleur de réseau et entre le contrôleur de réseau et les Agents d’hôte CN en cours d’exécution sur les ordinateurs hôtes Hyper-V.

 Le _Debug-ServiceFabricNodeStatus_ et _Get-NetworkControllerReplica_ applets de commande doit être exécutée à partir d’un des ordinateurs virtuels de nœud de contrôleur de réseau. Tous les autres applets de commande CN Diagnostic peuvent être exécutées à partir de n’importe quel hôte dispose d’une connectivité au contrôleur de réseau et qui est dans un groupe de sécurité de gestion de contrôleur de réseau (Kerberos) a accès au certificat X.509 pour la gestion du contrôleur de réseau. 
   
### <a name="hyper-v-host-diagnostics"></a>Diagnostic de l’hôte Hyper-V  
Ces applets de commande sont documentées sur TechNet dans le [rubrique d’applet de commande de Diagnostics de la virtualisation de réseau Hyper-V (HNV)](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/). Ils aident à identifier des problèmes dans le chemin de données entre les ordinateurs virtuels clients (est/ouest) et le trafic d’entrée par le biais d’une adresse IP virtuelle SLB (Nord/Sud). 

Le _Debug-VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, et _Test-EncapOverheadSettings_ sont tous les tests locales qui peuvent être exécutées à partir de n’importe quel ordinateur hôte Hyper-V. Les autres applets de commande appeler les tests de chemin d’accès de données par le biais du contrôleur de réseau et par conséquent ont besoin d’accéder au contrôleur de réseau comme descried ci-dessus.
 
### <a name="github"></a>GitHub
Le [référentiel GitHub de Microsoft/SDN](https://github.com/microsoft/sdn) possède un nombre d’exemples de scripts et workflows sur ces applets de commande de zone dans laquelle. En particulier, les scripts de diagnostic sont accessibles dans le [Diagnostics](https://github.com/Microsoft/sdn/diagnostics) dossier. Veuillez nous aider à contribuer à ces scripts en envoyant des demandes d’extraction.

## <a name="troubleshooting-workflows-and-guides"></a>Flux de travail et Guides de résolution des problèmes  

### <a name="hoster-validate-system-health"></a>[Hébergeur] Validation d’intégrité système
Il existe une ressource incorporée nommée _état de la Configuration_ dans plusieurs des ressources de contrôleur de réseau. État de configuration fournit des informations sur l’intégrité du système, y compris la cohérence entre la configuration du contrôleur de réseau et l’état réel (en cours d’exécution) sur les ordinateurs hôtes Hyper-V. 

Pour vérifier l’état de configuration, exécutez la commande suivante à partir de n’importe quel ordinateur hôte Hyper-V avec une connectivité au contrôleur de réseau.

>[!NOTE] 
>La valeur de la *NetworkController* paramètre doit être soit l’adresse IP ou le nom de domaine complet basé sur le nom de sujet de la X.509 > certificat créé pour le contrôleur de réseau.
>
>Le *Credential* paramètre ne doit être spécifié si le contrôleur de réseau à l’aide de l’authentification Kerberos (standard dans des déploiements VMM). Les informations d’identification doivent correspondre à un utilisateur qui se trouve dans le groupe de sécurité de gestion de contrôleur de réseau.

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

Vous trouverez ci-dessous un exemple de message d’état de Configuration:

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
> Il existe un bogue dans le système où les ressources d’Interface réseau pour la carte réseau de la machine virtuelle de SLB Mux Transit sont dans un état d’échec avec l’erreur «Virtuel commutateur – pas connecté au contrôleur d’hôte». Cette erreur peut être ignorée en toute sécurité si la configuration IP de la ressource VM NIC est définie sur une adresse IP à partir IP Pool du réseau logique Transit. Il existe un second bogue dans le système où les ressources d’Interface réseau pour les cartes réseau de la machine virtuelle de passerelle HNV fournisseur sont dans un état d’échec avec l’erreur «Virtual Switch – PortBlocked». Cette erreur peut également être ignorée si la configuration IP de la ressource VM NIC est définie sur null (par conception).


Le tableau ci-dessous répertorie les codes d’erreur, des messages et des actions de suivi à entreprendre en fonction de l’état de configuration observée.

  
| **Code**| **Message**| **Action**|  
|:--------:|:-----------:|----------:|  
| Inconnu| Erreur inconnue| |  
| HostUnreachable                       | L’ordinateur hôte n’est pas accessible | Vérifiez la connectivité réseau de gestion entre le contrôleur de réseau et l’ordinateur hôte |  
| PAIpAddressExhausted                  | Les adresses Ip fournisseur épuisée | Augmenter la taille du Pool IP du sous-réseau logique fournisseur HNV |  
| PAMacAddressExhausted                 | Les adresses Mac PA épuisée | Augmentation de la plage du Pool Mac |  
| PAAddressConfigurationFailure         | N’a pas pu monter les adresses de fournisseur à l’ordinateur hôte | Vérifiez la connectivité réseau de gestion entre le contrôleur de réseau et l’ordinateur hôte. |  
| CertificateNotTrusted                 | Certificat n’est pas approuvé  |Corrigez les certificats utilisés pour la communication avec l’ordinateur hôte. |  
| CertificateNotAuthorized              | Certificat non autorisé | Corrigez les certificats utilisés pour la communication avec l’ordinateur hôte. |  
| PolicyConfigurationFailureOnVfp       | Échec de la configuration des stratégies VFP | Il s’agit d’un échec d’exécution.  Aucune contournement définie. Collecter les journaux. |  
| PolicyConfigurationFailure            | Échec de pousser des stratégies pour les ordinateurs hôtes, en raison des échecs de communication ou d’autres erreurs dans le NetworkController.| Aucune action définie.  Il s’agit en raison de l’échec de l’état d’objectif de traitement dans les modules de contrôleur de réseau. Collecter les journaux. |  
| HostNotConnectedToController          | L’ordinateur hôte n’est pas encore connecté au contrôleur de réseau | Profil de port ne pas appliqué sur l’ordinateur hôte ou l’hôte n’est pas accessible à partir du contrôleur de réseau. Valider que la clé de Registre identifiant d’hôte correspond à l’ID d’Instance de la ressource de serveur |  
| MultipleVfpEnabledSwitches            | Il existe plusieurs VFp activé commutateurs sur l’ordinateur hôte  | Supprimez l’une des commutateurs, étant donné que l’Agent hôte du contrôleur de réseau prend uniquement en charge un commutateur virtuel avec l’extension VFP activée |  
| PolicyConfigurationFailure            | N’a pas pu distribuer les stratégies de réseau virtuel pour une carte réseau VM en raison d’erreurs de certificat ou des erreurs de connectivité  | Vérifiez si les certificats appropriés ont été déployées (nom objet du certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’ordinateur hôte avec le contrôleur de réseau |  
| PolicyConfigurationFailure            | N’a pas pu distribuer les stratégies de commutateur virtuel pour une carte réseau VM en raison d’erreurs de certificat ou des erreurs de connectivité  | Vérifiez si les certificats appropriés ont été déployées (nom objet du certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’ordinateur hôte avec le contrôleur de réseau|
| PolicyConfigurationFailure            | N’a pas pu distribuer les stratégies de pare-feu pour une carte réseau VM en raison d’erreurs de certificat ou des erreurs de connectivité | Vérifiez si les certificats appropriés ont été déployées (nom objet du certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’ordinateur hôte avec le contrôleur de réseau|
| DistributedRouterConfigurationFailure | Impossible de configurer les paramètres de routeur distribué sur la carte réseau virtuelle hôte                          | Erreur de la pile TCP/IP. Peut nécessiter de nettoyage de la carte réseau virtuelle PA et la récupération d’urgence Host sur le serveur sur lequel cette erreur a été signalée |
| DhcpAddressAllocationFailure          | Allocation d’adresses DHCP a échoué pour une carte réseau VM                                                    | Vérifiez si l’attribut d’adresse IP statique est configuré sur la ressource de cartes réseau |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Échec de connexion en raison d’erreurs réseau ou cert Mux | Vérifiez le numéro de code fourni dans le code de message d’erreur: cela correspond au code d’erreur winsock. Erreurs de certificat sont précis (par exemple, le certificat ne peut être vérifié, cert ne pas autorisé, etc..) |  
| HostUnreachable                       | Multiplexer est défectueux (le plus courant est BGPRouter déconnecté) | L’homologue BGP sur le RRAS (BGP virtuel) ou d’un commutateur Top-of-Rack (ToR) est inaccessible ou homologation pas correctement. Vérifier les paramètres de protocole BGP sur les ressources multiplexeur équilibrage de charge logicielle et de l’homologue BGP (ordinateur virtuel ToR ou RRAS) |  
| HostNotConnectedToController          | Agent d’ordinateur hôte SLB n’est pas connecté  | Vérifiez que le service SLB hôte Agent est en cours d’exécution; reportez-vous aux journaux de l’agent hôte SLB (auto en cours d’exécution) pour des raisons de pourquoi, au cas où SLBM (CN) a rejeté le certificat présenté par l’agent hôte exécutant état affiche des informations nuancées  |  
| PortBlocked                           | Le port VFP est bloqué, en raison du manque de réseau virtuel / stratégies ACL | Vérifiez si les autres erreurs, ce qui peuvent entraîner les stratégies n’est ne pas configuré. |  
| Surchargé                            | LoadBalancer MUX est surchargé.  | Problème de performances avec MUX |  
| RoutePublicationFailure               | LoadBalancer MUX n’est pas connecté à un routeur BGP | Vérifiez si le MUX dispose d’une connectivité avec les routeurs BGP et que l’homologation BGP est correctement configuré |  
| VirtualServerUnreachable              | LoadBalancer MUX n’est pas connecté au gestionnaire SLB | Vérifiez la connectivité entre SLBM et MUX |  
| QosConfigurationFailure               | Impossible de configurer des stratégies de QOS | Si une bande passante suffisante est disponible pour toutes les machines virtuelles si la réservation de qualité de service est utilisée |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Vérifiez la connectivité réseau entre le contrôleur de réseau et l’hôte Hyper-V (service d’Agent d’ordinateur hôte CN)
Exécutez le *netstat* commande ci-dessous pour valider qu’il existe trois connexions établie entre l’Agent hôte CN et les nœuds de contrôleur de réseau et qu’un seul socket d’écoute sur l’ordinateur hôte Hyper-V
- À l’écoute sur TCP:6640 de port sur l’hôte Hyper-V (Service d’Agent hôte CN)
- IP sur le port 6640 à CN nœud IP sur les ports éphémères (32000 >) de l’hôte deux connexions établies depuis Hyper-V
- Une connexion établie à partir d’IP de l’hôte Hyper-V sur le port éphémère à l’adresse IP reste de contrôleur de réseau sur le port 6640

>[!NOTE]
>Il peut être uniquement deux connexions établies sur un ordinateur hôte Hyper-V s’il n’y a aucun client ordinateurs virtuels déployés sur cet hôte particulier.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Vérifier les services Agent hôte
Le contrôleur de réseau communique avec les deux services de l’agent hôte sur les ordinateurs hôtes Hyper-V: SLB hôte Agent et CN hôte Agent. Il est possible qu’un ou l’autre de ces services ne fonctionne pas. Vérifiez leur état et redémarrer si elles s’exécutent pas.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Vérifier l’intégrité du contrôleur de réseau
S’il y a pas trois connexions établie ou si le contrôleur de réseau apparaît ne répond pas, vérifiez que tous les nœuds et les modules de service sont en cours d’exécution à l’aide des applets de commande suivantes. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Les modules de service du contrôleur réseau sont:
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

Vérifiez que ReplicaStatus est **prêt** et HealthState est **Ok**.

Dans la fabrication, le déploiement est avec un contrôleur de réseau à plusieurs nœuds, vous pouvez également vérifier le nœud de chaque service est principal sur et son état de réplication individuelles.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Vérifiez que l’état de réplication est prêt pour chaque service.
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Vérifier les identifiants d’hôte correspondant et certificats entre le contrôleur de réseau et chaque hôte Hyper-V 
Sur un ordinateur hôte Hyper-V, exécutez les commandes suivantes pour vérifier que l’identifiant d’hôte correspond à l’Id d’Instance d’une ressource de serveur sur le contrôleur de réseau

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

*Mise à jour* si SDNExpress à l’aide de scripts ou un déploiement manuel, mettre à jour la clé de l’identifiant d’hôte dans le Registre pour correspondre à l’Id d’Instance de la ressource de serveur. Redémarrez l’Agent d’hôte de contrôleur de réseau sur l’ordinateur hôte Hyper-V (serveur physique) si à l’aide de VMM, supprimez le serveur Hyper-V de VMM et supprimer la clé de Registre identifiant d’hôte. Ensuite, ajoutez à nouveau le serveur par le biais de VMM.


Vérifiez que leurs empreintes numériques des certificats X.509utilisés par l’ordinateur hôte Hyper-V (le nom d’hôte sera le nom du sujet du certificat le) pour la communication entre l’hôte Hyper-V (service d’Agent d’ordinateur hôte CN) et les nœuds de contrôleur de réseau (SouthBound) sont identiques. Vérifiez également que nom de sujet du certificat du reste du contrôleur de réseau *CN =<FQDN or IP>*.

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

Vous pouvez également vérifier les paramètres suivants de chaque certificat pour vous assurer que le nom du sujet est ce qui est prévu (nom d’hôte ou CN reste FQDN ou IP), le certificat n’a pas encore expiré, et que toutes les autorités de certification dans la chaîne de certificats sont incluses dans l’autorité racine approuvée.

- Nom du sujet  
- Date d’expiration  
- Approuvée par l’autorité racine  

*Mise à jour* si plusieurs certificats ont le même nom d’objet sur l’ordinateur hôte Hyper-V, l’Agent hôte du contrôleur de réseau choisit de manière aléatoire un à présenter au contrôleur de réseau. Cela peut ne pas correspond l’empreinte numérique de la ressource de serveur connue pour le contrôleur de réseau. Dans ce cas, supprimez l’un des certificats avec le même nom d’objet sur l’ordinateur hôte Hyper-V et redémarrez le service Agent hôte du contrôleur réseau. Si une connexion toujours pas possible, supprimez les autres certificats avec le même nom d’objet sur l’ordinateur hôte Hyper-V et supprimer la ressource de serveur correspondante dans VMM. Ensuite, recréez la ressource de serveur dans VMM qui génère un nouveau certificat X.509 et installez-le sur l’ordinateur hôte Hyper-V.
  

#### <a name="check-the-slb-configuration-state"></a>Vérifier l’état de Configuration SLB
L’état de Configuration SLB peut être déterminée dans le cadre de la sortie vers l’applet de commande Debug-NetworkController. Cette applet de commande génère également l’ensemble des ressources de contrôleur de réseau dans les fichiers JSON, toutes les configurations IP à partir de chaque ordinateur hôte Hyper-V (serveur) et la stratégie de réseau local à partir de tables de base de données de l’Agent hôte actuel. 

Traces supplémentaires seront collectées par défaut. Pour collecter les suivis ne pas, ajoutez le IncludeTraces-: paramètre $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>L’emplacement de sortie par défaut sera le répertoire \NCDiagnostics\ < répertoire_de_travail >. Le répertoire de sortie par défaut peut être modifié à l’aide de la `-OutputDirectory`paramètre. 

Vous trouverez les informations d’état de Configuration SLB dans le _diagnostics-slbstateResults.Json_ fichier dans ce répertoire.

Ce fichier JSON permettre être réparti dans les sections suivantes:
 * Ensemble fibre optique
   * SlbmVips - cette section répertorie l’adresse IP de l’adresse VIP SLB gestionnaire qui est utilisé par le contrôleur de réseau coodinate configuration et d’intégrité entre le SLB Muxes et les Agents d’hôte SLB.
   * MuxState - cette section répertorie une valeur pour chaque Mux SLB déployé donné à l’état de la mux
   * Configuration du routeur - cette section répertorie en amont du routeur (l’homologue BGP) numéro de système autonome (ASN), l’adresse IP de Transit et ID. Il répertorie également les SLB Muxes ASN et IP de Transit.
   * Connecté hôte Info - cette section liste l’adresse IP de gestion répondra à tous les hôtes Hyper-V pour exécuter des charges de travail avec équilibrage de charge.
   * Plages d’adresses IP virtuelles - cette section répertorie les plages de pool IP d’adresses IP virtuelles publiques et privées. L’adresse IP virtuelle SLBM seront inclus dans une adresse IP allouée à partir d’une de ces plages. 
   * Cette section répertorie MUX itinéraires - une valeur pour chaque Mux SLB déployé contenant toutes les annonces d’itinéraire pour ce mux particulier.
 * Client
   * VipConsolidatedState - cette section répertorie l’état de connectivité pour chaque adresse IP virtuelle du client, y compris le préfixe de l’itinéraire publié, l’hôte Hyper-V et les points de terminaison DIP.
    
> [!NOTE]
> État SLB peut être établie directement à l’aide du [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script disponible sur le [référentiel GitHub SDN de Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validation de la passerelle

**À partir du contrôleur de réseau:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**À partir d’ordinateurs virtuels de passerelle:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**À partir du haut de commutateur ToR (Rack):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Routeur BGP de Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

En plus, les problèmes que nous avons vu jusqu’ici (en particulier sur les déploiements en fonction de SDNExpress), à partir de la raison la plus courante pour compartiment client non mise en route configuré sur les ordinateurs virtuels GW semble être le fait que la capacité de GW dans FabricConfig.psd1 est inférieure par rapport à ce que gens essayez d’attribuer pour les connexions réseau (Tunnels S2S) TenantConfig.psd1. Cela peut être vérifié aisément en comparant les sorties des commandes suivantes:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Hébergeur] Valider le plan de données
Une fois que le contrôleur de réseau a été déployé, les réseaux virtuels clients et les sous-réseaux ont été créés et machines virtuelles ont été associés aux sous-réseaux virtuels, les tests de niveau supplémentaires de l’infrastructure peuvent être effectuées par l’hébergeur pour vérifier la connectivité du client.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Vérifiez la connectivité de réseau logique fournisseur HNV
Après l’invité premier ordinateur virtuel en cours d’exécution sur un ordinateur hôte Hyper-V a été connecté à un réseau virtuel locataire, le contrôleur de réseau attribuera deux adresses IP de fournisseur HNV (adresses IP de fournisseur) à l’ordinateur hôte Hyper-V. Ces adresses IP est proposé par Pool d’IP du réseau logique fournisseur HNV et être géré par le contrôleur de réseau.  Pour savoir quels sont ces deux adresses IP de HNV de

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

Ces adresses IP du fournisseur HNV (PA IPs) sont affectés aux cartes Ethernet créé dans un compartiment de réseau TCP/IP distinct et ont un nom de la carte de _VLANX_ où X est le réseau local virtuel affecté au réseau logique HNV fournisseur (transport).

La connectivité entre deux hôtes Hyper-V à l’aide du fournisseur de HNV réseau logique peut être effectuée par un test ping avec un compartiment supplémentaire (-c Y) paramètre où Y est le compartiment de réseau TCP/IP dans lequel les PAhostVNICs sont créés. Ce compartiment peut être déterminée en exécutant:

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
> Les cartes de carte réseau virtuelle hôte PA ne sont pas utilisés dans le chemin de données et par conséquent, vous n’ont pas d’une adresse IP attribuée à la carte «vEthernet (PAhostVNic)».

Par exemple, supposons qu’HNV fournisseur (PA) des adresses IP des ordinateurs hôtes Hyper-V 1 et 2:

|Ordinateur hôte hyper-V-|-Adresse IP de PA 1|-Adresse IP de PA 2|
|---             |---            |---             |
|Hôte 1 | 10.10.182.64 | 10.10.182.65 |
|Hôte 2 | 10.10.182.66 | 10.10.182.67 |

Nous pouvons ping entre les deux à l’aide de la commande suivante pour vérifier la connectivité de réseau logique fournisseur HNV.

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

*Mise à jour* ping si fournisseur HNV ne fonctionne pas, vérifiez la connectivité de votre réseau physique, y compris la configuration de réseau local virtuel. Les cartes réseau physiques sur chaque ordinateur hôte Hyper-V doivent être en mode trunk avec aucun réseau local virtuel spécifique affecté. La carte réseau virtuelle hôte de gestion doit être isolé du réseau logique gestion VLAN.

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Vérifiez la MTU et trame étendue prise en charge sur le réseau logique du fournisseur HNV

Un autre problème courant dans le réseau logique HNV fournisseur est que les ports de réseau physique et/ou de la carte Ethernet n’ont pas une MTU suffisamment grande configurée pour gérer la charge de l’encapsulation VXLAN (ou NVGRE). 
>[!NOTE]
> Certaines cartes Ethernet et les pilotes prennent en charge la nouvelle * mot clé EncapOverhead qui est automatiquement définis par l’Agent d’hôte de contrôleur de réseau à une valeur de 160. Cette valeur sera ensuite être ajoutée à la valeur de la * mot clé JumboPacket dont somme est utilisé comme la MTU publiée.
> Par exemple, * EncapOverhead = 160 et * JumboPacket = 1514 = > mTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Pour tester le réseau logique fournisseur HNV prend en charge la plus grande MTU taille de bout en bout ou non, utilisez le _Test-LogicalNetworkSupportsJumboPacket_ applet de commande:
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

# TODO: Success Results aftering updating MTU on physical switch ports

```

*Mise à jour*
* Ajuster la taille de la MTU sur les ports de commutateur physique pour être d’au moins 1674B (y compris 14C Ethernet en-tête et le code)
* Si votre carte réseau ne prend pas en charge le mot clé EncapOverhead, ajustez le mot clé JumboPacket pour être d’au moins 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Vérifiez la connectivité des cartes réseau de machine virtuelle locataire
Chaque carte réseau virtuelle affectée à un ordinateur virtuel invité a un mappage de l’autorité de certification-PA entre l’adresse privée client (CA) et l’espace d’adresse de fournisseur de HNV (PA). Ces mappages sont conservés dans les tableaux de serveur OVSDB sur chaque ordinateur hôte Hyper-V et sont accessibles par l’exécution de l’applet de commande suivant.

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
> Si les mappages d’autorité de certification-PA vous attendez ne sont pas de sortie pour un client donné, ordinateur virtuel, vérifiez les ressources NIC VM et la Configuration IP sur le contrôleur de réseau à l’aide de la _Get-NetworkControllerNetworkInterface_ applet de commande. Vérifiez également les connexions établies entre les nœuds CN hôte Agent et le contrôleur de réseau.

Avec cette information, une commande ping sur des ordinateurs virtuels client peut maintenant être initiée par l’hébergeur du contrôleur de réseau à l’aide de la _Test-VirtualNetworkConnection_ applet de commande.

## <a name="specific-troubleshooting-scenarios"></a>Scénarios de résolution des problèmes spécifiques

Les sections suivantes fournissent des conseils pour résoudre des scénarios spécifiques.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Aucune connectivité réseau entre deux ordinateurs virtuels clients

1.  [Client] Vérifiez que le pare-feu Windows dans les machines virtuelles clientes ne bloque pas le trafic.  
2.  [Client] Vérifiez que les adresses IP affectées à l’ordinateur virtuel client en exécutant _ipconfig_. 
3.  [Hébergeur] Exécutez **Test-VirtualNetworkConnection** à partir de l’ordinateur hôte Hyper-V pour valider la connectivité entre les deux ordinateurs virtuels clients en question. 

>[!NOTE]
>Le VSID fait référence à l’ID de sous-réseau virtuel. Dans le cas VXLAN, il s’agit de l’identificateur de réseau VXLAN (VNI). Vous pouvez trouver cette valeur en exécutant la **Get-PACAMapping** applet de commande.

#### <a name="example"></a>Exemple

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Créez autorité de certification-ping entre «Ordinateur virtuel 1 vert Web» avec IP SenderCA de 192.168.1.4 sur hôte «sa18n30-2.sa18.nttest.microsoft.com» avec IP Mgmt de 10.127.132.153 ListenerCA IP de 192.168.1.5 attachée au sous-réseau virtuel (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Test de ping espace à l’autorité de certification de démarrage à partir de la session de suivi Ping pour 192.168.1.5 a réussi à partir de l’adresse 192.168.1.4 Rtt = 0ms


Informations de routage autorité de certification:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informations de routage PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [Client] Vérifiez qu’il n’existe aucune stratégie de pare-feu distribué spécifié sur le sous-réseau virtuel ou les interfaces réseau de machine virtuelle qui seraient de bloquer le trafic.    

L’API REST de contrôleur de réseau trouvée dans l’environnement de démonstration à sa18n30nc dans le domaine sa18.nttest.microsoft.com de requête.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examinez la Configuration IP et des sous-réseaux virtuels qui font référence à cette liste ACL

1. [Hébergeur] Exécutez ``Get-ProviderAddress``sur les deux Hyper-V qui héberge les deux ordinateurs hôtes d’ordinateurs virtuels en question de client, puis exécutez ``Test-LogicalNetworkConnection``ou ``ping -c <compartment>``à partir de l’ordinateur hôte Hyper-V pour valider la connectivité sur le réseau logique fournisseur HNV
2.  [Hébergeur] Assurez-vous que les paramètres MTU sont correctes sur les ordinateurs hôtes Hyper-V et toute couche 2périphériques entre les hôtes Hyper-V de commutation. Exécutez ``Test-EncapOverheadValue``sur tous les ordinateurs hôtes Hyper-V en question. Vérifiez également que tous les commutateurs de couche 2 entre les deux ont MTU la valeur octets 1674moins pour prendre en compte pour la surcharge maximale de 160octets.  
3.  [Hébergeur] Si des adresses IP fournisseur ne sont pas présents et/ou la connectivité de l’autorité de certification est interrompue, vérifiez la stratégie réseau a été reçue. Exécutez ``Get-PACAMapping``pour voir si les règles de l’encapsulation et les mappages d’autorité de certification-PA requis pour la création de réseaux virtuels de superposition sont établis correctement.  
4.  [Hébergeur] Vérifiez que l’Agent hôte du contrôleur de réseau est connecté au contrôleur de réseau. Exécutez ``netstat -anp tcp |findstr 6640``pour voir si   
5.  [Hébergeur] Vérifiez que l’ID de l’ordinateur hôte dans HKLM / correspond à l’ID d’Instance des ressources du serveur qui héberge les ordinateurs virtuels clients.  
6. [Hébergeur] Vérifiez que l’ID du profil de Port correspond à l’ID d’Instance des Interfaces de réseau d’ordinateurs virtuels des ordinateurs virtuels client.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>La journalisation, le suivi et diagnostics avancés

Les sections suivantes fournissent des informations sur les diagnostics avancés, journalisation et suivi.

### <a name="network-controller-centralized-logging"></a>Contrôleur de réseau centralisée de journalisation 
 
Le contrôleur de réseau peut automatiquement collecter les journaux du débogueur et les stocker dans un emplacement centralisé. Collecte de journaux peut être activée quand lorsque vous déployez le contrôleur de réseau pour la première fois ou à tout moment ultérieurement. Les journaux sont collectés à partir du contrôleur de réseau et les éléments gérés par le contrôleur de réseau du réseau: hôte machines, les équilibreurs de charge logicielle (SLB) et les ordinateurs virtuels de passerelle. 

Ces journaux incluent les journaux de débogage pour le cluster de contrôleur de réseau, l’application de contrôleur de réseau, les journaux de passerelle, SLB, un réseau virtuel et le pare-feu distribué. Chaque fois qu’une nouvelle hôte/SLB/la passerelle est ajoutée au contrôleur de réseau, la journalisation est démarrée sur ces ordinateurs. De même, lorsqu’une ordinateur hôte/SLB/passerelle est supprimée à partir du contrôleur de réseau, la journalisation est arrêtée sur ces ordinateurs.

#### <a name="enable-logging"></a>Activer la journalisation

La journalisation est activée automatiquement lorsque vous installez le cluster de contrôleur de réseau à l’aide de la **Install-NetworkControllerCluster** applet de commande. Par défaut, les journaux sont collectés localement sur les nœuds de contrôleur de réseau à *%systemdrive%\SDNDiagnostics*. Il est **fortement recommandé de** modifier cet emplacement pour être un partage de fichiers distant (non local). 

Les journaux de cluster du contrôleur de réseau sont stockés à *%programData%\Windows Fabric\log\Traces*. Vous pouvez spécifier un emplacement centralisé pour la collecte de journaux avec le **DiagnosticLogLocation** paramètre avec la recommandation qu’il s’agit également être un partage de fichiers distant. 

Si vous souhaitez limiter l’accès à cet emplacement, vous pouvez fournir les informations d’identification de l’accès avec le **LogLocationCredential** paramètre. Si vous fournissez les informations d’identification pour accéder à l’emplacement du journal, vous devez également fournir le **CredentialEncryptionCertificate** paramètre, qui est utilisée pour chiffrer les informations d’identification stockées localement sur les nœuds de contrôleur de réseau.  

Avec les paramètres par défaut, il est recommandé d’avoir au moins 75Go d’espace libre dans le site central, 25Go sur les nœuds locales (si vous N'utilisez pas un emplacement central) pour un cluster de contrôleur de réseau de 3nœuds.

#### <a name="change-logging-settings"></a>Modifier les paramètres de journalisation

Vous pouvez modifier les paramètres de journalisation à tout moment à l’aide de la ``Set-NetworkControllerDiagnostic``applet de commande. Peuvent être modifiés les paramètres suivants:

- **Emplacement du journal centralisé**.  Vous pouvez modifier l’emplacement pour stocker tous les journaux, avec le ``DiagnosticLogLocation``paramètre.  
- **Informations d’identification pour accéder à l’emplacement du journal**.  Vous pouvez modifier les informations d’identification pour accéder à l’emplacement du journal, avec la ``LogLocationCredential``paramètre.  
- **Déplacer vers l’enregistrement local**.  Si vous avez spécifié un emplacement centralisé pour stocker les journaux, vous pouvez ramener à une session localement les nœuds du contrôleur de réseau avec la ``UseLocalLogLocation``paramètre (non recommandé en raison de l’espace disque élevé requis).  
- **Étendue de l’enregistrement**.  Par défaut, tous les journaux sont collectés. Vous pouvez modifier l’étendue pour collecter les journaux de cluster de contrôleur de réseau uniquement.  
- **Niveau de journalisation**.  Le niveau d’enregistrement par défaut est information. Vous pouvez la modifier pour Verbose, avertissement ou erreur.  
- **Journal chronologie temps**.  Les journaux sont stockés de façon circulaire. Vous aurez 3jours de données de journalisation par défaut, que vous utilisiez journalisation locale ou centralisée. Vous pouvez modifier ce délai avec **LogTimeLimitInDays** paramètre.  
- **Chronologie de taille de journal**.  Par défaut, vous devez un maximum 75Go de données de journalisation à l’aide de journalisation centralisée et 25Go si vous utilisez la journalisation locale. Vous pouvez modifier cette limite avec la **LogSizeLimitInMBs** paramètre.

#### <a name="collecting-logs-and-traces"></a>Collecte de journaux et des traces

Déploiements VMM utilisent la journalisation centralisée pour le contrôleur de réseau par défaut. L’emplacement du partage de fichier pour ces fichiers journaux sont spécifié lorsque vous déployez le modèle de service du contrôleur de réseau.

Si un emplacement de fichier n’a pas été spécifié, local journalisation servira sur chaque nœud du contrôleur de réseau avec des journaux enregistrés sous C:\Windows\tracing\SDNDiagnostics. Ces journaux sont enregistrés à l’aide de la hiérarchie suivante:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Traces

Le contrôleur de réseau utilise la structure de Service (Azure). Service Fabric journaux peuvent être nécessaires lors du dépannage de certains problèmes. Ces journaux sont accessibles sur chaque nœud du contrôleur de réseau à C:\ProgramData\Microsoft\Service Fabric.

Si un utilisateur a exécuté le _Debug-NetworkController_ applet de commande, des journaux supplémentaires seront disponibles sur chaque ordinateur hôte Hyper-V qui a été spécifié avec une ressource de serveur dans le contrôleur de réseau. Ces journaux (et les traces s’il est activé) sont conservés sous C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostics SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erreurs SLBM Fabric (actions de fournisseur d’hébergement service)

1.  Vérifiez que le Gestionnaire d’équilibrage de charge logiciels (SLBM) fonctionne et que les couches d’orchestration peuvent communiquer entre eux: SLBM-& gt; sLB Mux et SLBM-& gt; sLB hôte Agents. Exécutez [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) à partir de n’importe quel nœud d’accéder au point de terminaison REST de contrôleur de réseau.  
2.  Valider la *SDNSLBMPerfCounters* dans l’Analyseur de performances sur un du nœud du contrôleur de réseau machines virtuelles (de préférence le contrôleur de réseau nœud principal - Get-NetworkControllerReplica):
    1.  Moteur d’équilibrage de charge (kg) est connecté à SLBM? (*SLBM LBEngine Configurations Total* > 0)  
    2.  SLBM au moins connaît ses propres points de terminaison? (*Total de points de terminaison VIP* > = 2)  
    3.  Ordinateurs hôtes Hyper-V (DIP) sont connectés à SLBM? (*Clients HP connectés* == num serveurs)   
    4.  SLBM est connecté à Muxes? (*Muxes connecté* == *Muxes intègre sur SLBM* == *Muxes reporting sain* = # SLB Muxes VM).  
3.  Vérifiez le routeur BGP configuré est correctement homologation avec le MUX SLB  
    1.  Si vous utilisez RRAS avec accès à distance (c'est-à-dire BGP virtuel):  
        1.  Get-BgpPeer doivent afficher connecté  
        2.  Get-BgpRouteInformation doit afficher au moins un itinéraire pour le SLBM self VIP  
    2.  Si vous utilisez physique Top-of-Rack (ToR) commutateur en tant que l’homologue BGP, consultez la documentation  
        1.  Par exemple: # afficher instance bgp  
4.  Valider la *SlbMuxPerfCounters* et *SLBMUX* compteurs PerfMon sur l’ordinateur virtuel Mux de SLB
5.  Vérifiez l’état de configuration et les plages d’adresses IP virtuelles dans le Gestionnaire d’équilibrage de charge logicielle ressource  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| ConvertTo-json-profondeur 8 (Vérifiez les plages d’adresses IP virtuelles dans des pools d’adresses IP et assurez-vous que SLBM self-VIP (*LoadBalanacerManagerIPAddress*) et les adresses IP côté client se trouvent dans ces plages)  
        1. Get-NetworkControllerIpPool - NetworkId «< publique/privée VIP logique ressource ID réseau >» - SubnetId «< publique/privée VIP logique ressource ID de sous-réseau >» ResourceId-«<IP Pool Resource Id>» - ConnectionUri $uri | convertto-json-profondeur 8 
    2.  Debug-NetworkControllerConfigurationState-  

Si un des contrôles ci-dessus échouent, le client état SLB sera également dans un mode d’échec.  

**Mise à jour**   
Selon les informations de diagnostic suivantes présentées, les erreurs suivantes:  
* Assurez-vous que SLB multiplexeur sont connectés.  
  * Résoudre les problèmes de certificat  
  * Résoudre les problèmes de connectivité réseau  
* Vérifiez les informations homologation BGP sont correctement configurées.  
* Assurez-vous d’ID d’hôte dans le Registre correspond à l’ID d’Instance de serveur de ressources du serveur (référence annexe pour *HostNotConnected* code d’erreur)  
* Collecter les journaux  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erreurs de client SLBM (actions client et fournisseur de service hébergement)

1.  [Hébergeur] Vérifiez *Debug-NetworkControllerConfigurationState* pour voir si toutes les ressources LoadBalancer sont dans un état d’erreur. Essayez de réduire en suivant les éléments d’Action Table dans l’annexe.   
    1.  Vérifiez qu’un point de terminaison VIP est présent et itinéraires de publicité  
    2.  Vérifiez les points de terminaison DIP combien ont été découverts pour le point de terminaison VIP  
2.  [Client] Valider les ressources d’équilibrage de charge sont spécifiés correctement  
    1.  Valider DIP les points de terminaison qui sont enregistrés dans SLBM hébergent des ordinateurs virtuels clients qui correspondent aux configurations IP adresse principale LoadBalancer pool  
3.  [Hébergeur] Si les points de terminaison DIP ne sont pas découverts ou connectés:   
    1.  Vérifiez *NetworkControllerConfigurationState de débogage*  
        1.  Valider que CN et SLB Agent d’ordinateur hôte est correctement connecté au coordinateur d’événement de contrôleur de réseau à l’aide ``netstat -anp tcp |findstr 6640)``  
    2.  Vérifiez *identifiant d’hôte* dans *nchostagent* service la clé de Registre (référence *HostNotConnected* code d’erreur dans l’annexe) correspond à l’Id d’instance de la ressource de serveur correspondante (``Get-NCServer |convertto-json -depth 8``)  
    3.  Vérifier l’id de profil de port pour le port de la machine virtuelle correspond à l’Id d’Instance de ressource correspondante virtuels cartes réseau   
4.  [Fournisseur d’hébergement] Collecter les journaux   

#### <a name="slb-mux-tracing"></a>Suivi de SLB Mux

Informations à partir de la Muxes équilibrage de charge logicielle peuvent également être déterminées par le biais de l’Observateur d’événements. 
1. Cliquez sur «Afficher et de déboguer les journaux d’analyse» dans le menu Affichage de l’Observateur d’événements
2. Accédez à «Journaux des Applications et Services» > microsoft > windows > slbMuxDriver > dans l’Observateur d’événements de Trace 
3. Cliquez avec le bouton droit sur ce dernier et sélectionnez «Activer le journal»

>[!NOTE]
>Il est recommandé de disposer uniquement cette journalisation activée pendant un court instant alors que vous essayez de reproduire un problème

### <a name="vfp-and-vswitch-tracing"></a>VFP et suivi du commutateur virtuel

À partir de n’importe quel Hyper-V d’hôte qui héberge un ordinateur virtuel attaché à un réseau virtuel locataire invité, vous pouvez collectées une trace VFP pour déterminer où potentielle des problèmes.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```

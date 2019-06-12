---
title: Résoudre les problèmes de la pile de mise en réseau SDN (Software Defined Networking) dans Windows Server
description: Ce guide de Windows Server examine les erreurs courantes de mise en réseau SDN (Software Defined) et les scénarios d’échec et présente un flux de travail de résolution des problèmes qui s’appuie sur les outils de diagnostics disponibles.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: eeb0c335e4afd3c6835a04421a15073aeab6cdc6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446241"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Résoudre les problèmes de la pile de mise en réseau SDN (Software Defined Networking) dans Windows Server

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Ce guide examine les erreurs courantes de mise en réseau SDN (Software Defined) et les scénarios d’échec et présente un flux de travail de résolution des problèmes qui s’appuie sur les outils de diagnostics disponibles.  

Pour plus d’informations sur Microsoft Software-Defined Networking, consultez [Sdn](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Types d’erreurs  
La liste suivante représente la classe de problèmes le plus souvent avec la virtualisation de réseau Hyper-V (HNVv1) dans Windows Server 2012 R2 à partir des déploiements de production de commercialisé et coïncide de nombreuses manières avec les mêmes types de problèmes dans Windows Server 2016 HNVv2 avec la nouvelle pile de réseau SDN (Software Defined).  

La plupart des erreurs peuvent être classés dans un petit ensemble de classes :   
* **Configuration non valide ou non pris en charge**  
   Un utilisateur appelle l’API NorthBound ou de façon incorrecte avec stratégie non valide.   

* **Erreur dans l’application de la stratégie**  
     Stratégie à partir du contrôleur de réseau n’a pas été remis à un hôte Hyper-V, et / considérablement retardé ou non à jour sur tous les hôtes Hyper-V (par exemple, après une Migration dynamique).  
* **Bogue de logiciels ou les dérives de configuration**  
  Problèmes de chemin d’accès de données résultant dans les paquets ignorés.  

* **Erreur externe lié au matériel de carte réseau / pilotes ou la sous-couche réseau fabric**  
  Dysfonctionnement déchargements de tâche (par exemple, VMQ) ou de structure de réseau de sous-couche mal configuré (par exemple, MTU)   

  Ce guide de dépannage examine chacune de ces catégories d’erreur et recommande les meilleures pratiques et des outils de diagnostic disponibles pour identifier et résoudre l’erreur.  

## <a name="diagnostic-tools"></a>Outils de diagnostic  

Avant d’aborder la résolution des problèmes des flux de travail pour chacun de ces types d’erreurs, nous allons examiner les outils de diagnostic.   

Pour utiliser les outils de diagnostic de contrôleur de réseau (chemin de contrôle), vous devez tout d’abord installer la fonctionnalité de RSAT-NetworkController et importer le ``NetworkControllerDiagnostics`` module :  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Pour utiliser les outils de diagnostic HNV Diagnostics (chemin de données), vous devez importer le ``HNVDiagnostics`` module :

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnostic de contrôleur de réseau  
Ces applets de commande sont documentées sur TechNet dans le [rubrique d’applet de commande de diagnostic de contrôleur de réseau](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Ils permettent d’identifier les problèmes de cohérence de stratégie de réseau dans le contrôle-chemin d’accès entre les nœuds du contrôleur de réseau et entre le contrôleur de réseau et les Agents d’hôte NC en cours d’exécution sur les ordinateurs hôtes Hyper-V.

 Le _Debug-ServiceFabricNodeStatus_ et _Get-NetworkControllerReplica_ applets de commande doit être exécutée à partir d’une des machines virtuelles de nœud de contrôleur de réseau. Tous les autres applets de commande de Diagnostic de contrôleur de réseau peuvent être exécutés à partir de n’importe quel hôte dispose d’une connectivité au contrôleur de réseau et qui est dans un groupe de sécurité de gestion du contrôleur réseau (Kerberos) accès au certificat X.509 permettant de gérer le contrôleur de réseau. 

### <a name="hyper-v-host-diagnostics"></a>Diagnostics de l’hôte Hyper-V  
Ces applets de commande sont documentées sur TechNet dans le [rubrique d’applet de commande de diagnostic de la virtualisation de réseau Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Ils aident à identifier des problèmes dans le chemin de données entre les machines virtuelles de locataire (est/ouest) et le trafic en entrée via une adresse IP virtuelle SLB (Nord/Sud). 

Le _Debug-VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, et _Test-EncapOverheadSettings_ sont tous les tests locaux qui peuvent être exécutées à partir de n’importe quel hôte Hyper-V. Les autres applets de commande appeler des tests de chemin d’accès de données via le contrôleur de réseau et par conséquent ont besoin d’accéder au contrôleur de réseau en tant que descried ci-dessus.

### <a name="github"></a>GitHub
Le [référentiel de GitHub Microsoft/SDN](https://github.com/microsoft/sdn) a un nombre d’exemples de scripts et les workflows qui appuient sur ces applets de commande d’origine. En particulier, les scripts de diagnostics figurent dans le [Diagnostics](https://github.com/Microsoft/sdn/diagnostics) dossier. Aidez-nous à contribuer à ces scripts en soumettant des requêtes d’extraction.

## <a name="troubleshooting-workflows-and-guides"></a>Résolution des problèmes de flux de travail et Guides  

### <a name="hoster-validate-system-health"></a>[Hébergeur] Valider l’intégrité du système
Il existe une ressource incorporée nommée _état de Configuration_ dans plusieurs des ressources du contrôleur de réseau. État de la configuration fournit des informations sur l’intégrité du système, notamment la cohérence entre la configuration du contrôleur de réseau et l’état réel (en cours d’exécution) sur les ordinateurs hôtes Hyper-V. 

Pour vérifier l’état de configuration, exécutez la commande suivante à partir de n’importe quel hôte Hyper-V avec une connectivité au contrôleur de réseau.

>[!NOTE] 
>La valeur de la *NetworkController* paramètre doit être soit le nom de domaine complet ou adresse IP en fonction du nom de sujet de X.509 > certificat créé pour le contrôleur de réseau.
>
>Le *informations d’identification* le paramètre doit uniquement être spécifié si le contrôleur de réseau à l’aide de l’authentification Kerberos (standard dans les déploiements de VMM). Les informations d’identification doivent correspondre à un utilisateur qui est dans le groupe de sécurité réseau contrôleur de gestion.

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

Vous trouverez ci-dessous un exemple de message d’état de Configuration :

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
> Il existe un bogue dans le système où les ressources de l’Interface réseau pour la carte réseau de la machine virtuelle de SLB Mux Transit sont dans un état d’échec avec l’erreur « Virtuel commutateur – pas connecté au contrôleur hôte ». Cette erreur peut être ignorée en toute sécurité si la configuration IP dans la ressource VM NIC est définie sur une adresse IP à partir Pool du réseau logique de Transit d’adresses IP. Il existe un second bogue dans le système où les ressources de l’Interface réseau pour les cartes réseau de la machine virtuelle de passerelle HNV fournisseur sont dans un état d’échec avec l’erreur « Virtual Switch – PortBlocked ». Cette erreur peut également être ignorée si la configuration IP dans la ressource VM NIC est définie sur null (par conception).


Le tableau ci-dessous répertorie les codes d’erreur, les messages et les actions de suivi à entreprendre en fonction de l’état de configuration observé.


| **Code**| **Message**| **Action**|  
|--------|-----------|----------|  
| Inconnu| Erreur inconnue| |  
| HostUnreachable                       | L’ordinateur hôte n’est pas accessible | Vérifiez la connectivité de réseau de gestion entre le contrôleur de réseau et un hôte |  
| PAIpAddressExhausted                  | Les adresses Ip fournisseur épuisée | Augmenter la taille du Pool IP du sous-réseau logique de fournisseur HNV |  
| PAMacAddressExhausted                 | Les adresses Mac de PA épuisée | Augmenter la plage du Pool Mac |  
| PAAddressConfigurationFailure         | Échec de l’analyse les adresses de fournisseur à l’hôte | Vérifiez la connectivité de réseau de gestion entre le contrôleur de réseau et un hôte. |  
| CertificateNotTrusted                 | Certificat n’est pas approuvé  |Corrigez les certificats utilisés pour la communication avec l’hôte. |  
| CertificateNotAuthorized              | Certificat non autorisé | Corrigez les certificats utilisés pour la communication avec l’hôte. |  
| PolicyConfigurationFailureOnVfp       | Échec de la configuration de stratégies VFP | Il s’agit d’un échec d’exécution.  Aucun contournement définie. Collecter les journaux. |  
| PolicyConfigurationFailure            | Échec en envoyant des stratégies pour les ordinateurs hôtes, en raison d’échecs de communication ou d’autres erreurs dans le NetworkController.| Aucune action définie.  Il s’agit en raison d’une défaillance dans l’état de l’objectif de traitement dans les modules de contrôleur de réseau. Collecter les journaux. |  
| HostNotConnectedToController          | L’hôte n’est pas encore connecté au contrôleur de réseau | Profil de port ne pas appliqué sur l’ordinateur hôte ou l’hôte n’est pas accessible à partir du contrôleur de réseau. Valider que la clé de Registre HostID correspond à l’ID d’Instance de la ressource de serveur |  
| MultipleVfpEnabledSwitches            | Il existe plusieurs VFp activée commutateurs sur l’hôte  | Supprimer l’un des commutateurs, étant donné que l’Agent hôte du contrôleur réseau prend uniquement en charge un commutateur virtuel avec l’extension VFP activée |  
| PolicyConfigurationFailure            | Impossible de distribuer les stratégies de réseau virtuel pour un réseau d’ordinateur virtuel en raison d’erreurs de certificat ou des erreurs de connectivité  | Vérifiez si les certificats appropriés ont été déployés (nom de sujet de certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau |  
| PolicyConfigurationFailure            | Impossible de distribuer les stratégies de commutateur virtuel pour un réseau d’ordinateur virtuel en raison d’erreurs de certificat ou des erreurs de connectivité  | Vérifiez si les certificats appropriés ont été déployés (nom de sujet de certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau|
| PolicyConfigurationFailure            | Impossible de distribuer les stratégies de pare-feu pour un réseau d’ordinateur virtuel en raison d’erreurs de certificat ou des erreurs de connectivité | Vérifiez si les certificats appropriés ont été déployés (nom de sujet de certificat doit correspondre à nom de domaine complet de l’hôte). Vérifiez également la connectivité de l’hôte avec le contrôleur de réseau|
| DistributedRouterConfigurationFailure | Impossible de configurer les paramètres du routeur distribué sur la carte réseau virtuelle hôte                          | Erreur de pile TCP/IP. Peut nécessiter de nettoyer les cartes réseau virtuelles PA et récupération d’urgence Host sur le serveur sur lequel cette erreur a été signalée |
| DhcpAddressAllocationFailure          | Échec de l’allocation d’adresses DHCP pour un réseau d’ordinateur virtuel                                                    | Vérifiez si l’attribut d’adresse IP statique est configuré sur la ressource de carte réseau |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Impossible de se connecter à Mux en raison d’erreurs réseau ou de certificat | Vérifiez le code numérique fourni dans le code de message d’erreur : cela correspond au code d’erreur winsock. Erreurs de certificat sont précis (par exemple, certificat ne peut pas être vérifié, les certificats non autorisés, etc..) |  
| HostUnreachable                       | MUX est défectueux (le cas courant est BGPRouter déconnecté) | L’homologue BGP sur le commutateur de Top-of-Rack (ToR) ou le RRAS (machine virtuelle BGP) est inaccessible ou homologation pas correctement. Vérifiez les paramètres de BGP sur la ressource multiplexeur équilibreur de charge logiciel et de l’homologue BGP (machine virtuelle ToR ou RRAS) |  
| HostNotConnectedToController          | Agent hôte SLB n’est pas connecté.  | Vérifiez que le service SLB hôte Agent est en cours d’exécution ; Reportez-vous aux journaux d’agent hôte SLB (auto en cours d’exécution) pour des raisons de pourquoi, au cas où SLBM (NC) a rejeté le certificat présenté par l’agent hôte en cours d’exécution état affiche des informations subtils  |  
| PortBlocked                           | Le port VFP est bloqué, en raison du manque de réseau virtuel / stratégies ACL | Vérifiez si toutes les autres erreurs, ce qui peuvent provoquer les stratégies à n’être ne pas configuré. |  
| Surchargé                            | LoadBalancer MUX est surchargé.  | Problème de performances avec MUX |  
| RoutePublicationFailure               | LoadBalancer MUX n’est pas connecté à un routeur BGP | Vérifiez si le MUX dispose d’une connectivité avec les routeurs BGP et que l’homologation BGP est le programme d’installation correctement |  
| VirtualServerUnreachable              | LoadBalancer MUX n’est pas connecté au gestionnaire SLB | Vérifiez la connectivité entre SLBM et MUX |  
| QosConfigurationFailure               | Impossible de configurer des stratégies de QOS | Si une bande passante suffisante est disponible pour toutes les machines virtuelles si la réservation de qualité de service est utilisée |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Vérifiez la connectivité réseau entre le contrôleur de réseau et l’hôte Hyper-V (service de contrôleur de réseau hôte Agent)
Exécutez le *netstat* commande ci-dessous pour valider qu’il n’y a trois connexions établi entre l’Agent hôte de contrôleur de réseau et les nœuds de contrôleur de réseau et qu’un seul socket à l’écoute sur l’ordinateur hôte Hyper-V
- À l’écoute sur TCP:6640 de port sur l’hôte Hyper-V (Service d’Agent hôte CN)
- IP sur le port 6640 vers l’adresse IP du nœud de contexte de nommage sur les ports éphémères (32000 >) de l’hôte deux connexions établies à partir d’Hyper-V
- Une connexion établie à partir de l’IP de l’hôte Hyper-V sur le port éphémère à adresse IP REST de contrôleur de réseau sur le port 6640

>[!NOTE]
>Il peut être uniquement deux connexions établies sur un ordinateur hôte Hyper-V s’il n’existe aucune machine virtuelle de locataire déployé sur cet hôte particulier.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Vérifier les services de l’Agent hôte
Le contrôleur de réseau communique avec deux services de l’agent hôte sur les ordinateurs hôtes Hyper-V : SLB hôte Agent et l’Agent d’hôte de contrôleur de réseau. Il est possible qu’une ou les deux de ces services ne fonctionne pas. Vérifiez leur état et redémarrer si elles n’exécutent pas.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Vérifier l’intégrité du contrôleur de réseau
S’il en existe pas trois connexions établi ou si le contrôleur de réseau semble ne pas répondre, vérifiez que tous les nœuds et les modules de service sont en cours d’exécution à l’aide des applets de commande suivantes. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Les modules de service de contrôleur de réseau sont :
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

Vérifiez que le ReplicaStatus est **prêt** et HealthState est **Ok**.

Dans une production déploiement est un contrôleur de réseau à plusieurs nœuds, vous pouvez également vérifier quel nœud chaque service est principal sur et son état de réplica individuel.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Vérifiez que l’état de réplica est prêt pour chaque service.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Recherchez les identifiants d’hôte correspondante et les certificats entre le contrôleur de réseau et chaque hôte Hyper-V 
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

*Mise à jour* si SDNExpress à l’aide de scripts ou un déploiement manuel, mettre à jour la clé HostId dans le Registre pour faire correspondre l’Id d’Instance de la ressource de serveur. Redémarrez l’Agent de hôte du contrôleur de réseau sur l’hôte Hyper-V (serveur physique) si l’aide de VMM, supprimez le serveur Hyper-V à partir de VMM et supprimer la clé de Registre HostId. Puis, ajoutez de nouveau le serveur via VMM.


Vérifiez que les empreintes numériques des certificats X.509 utilisés par l’hôte Hyper-V (le nom d’hôte sera le nom du sujet du certificat) pour la communication (SouthBound) entre l’hôte Hyper-V (service de contrôleur de réseau hôte Agent) et les nœuds de contrôleur de réseau sont les mêmes. Vérifiez également que nom du sujet du certificat du reste du contrôleur de réseau *CN =<FQDN or IP>* .

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

Vous pouvez également vérifier les paramètres suivants de chaque certificat de s’assurer que le nom du sujet est ce qui est attendu (nom d’hôte ou nom de domaine complet NC REST ou IP), le certificat n’a pas encore expiré, et que toutes les autorités de certification dans la chaîne de certificats sont incluses dans la racine de confiance autorité.

- Nom de sujet  
- Date d'expiration  
- Approuvé par l’autorité racine  

*Mise à jour* si plusieurs certificats ont le même nom d’objet sur l’ordinateur hôte Hyper-V, l’Agent hôte du contrôleur de réseau choisit de manière aléatoire un à présenter au contrôleur de réseau. Cela peut ne pas correspondre à l’empreinte numérique de la ressource serveur connue pour le contrôleur de réseau. Dans ce cas, supprimez un des certificats avec le même nom d’objet sur l’ordinateur hôte Hyper-V et puis redémarrez le service de l’Agent hôte du contrôleur réseau. Si une connexion toujours pas possible, supprimez l’autre certificat portant le même nom de sujet sur l’ordinateur hôte Hyper-V et la ressource de serveur correspondante dans VMM. Ensuite, recréer la ressource de serveur dans VMM qui génère un nouveau certificat X.509 et installez-le sur l’ordinateur hôte Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Vérifier l’état de Configuration SLB
Vous pouvez déterminer l’état de Configuration SLB en tant que partie de la sortie vers l’applet de commande Debug-NetworkController. Cette applet de commande génère également l’ensemble actuel des ressources du contrôleur de réseau dans les fichiers JSON, toutes les configurations IP à partir de chaque hôte Hyper-V (serveur) et stratégie de réseau local à partir des tables de base de données de l’Agent hôte. 

Traces supplémentaires seront collectés par défaut. Pour ne pas collecter les traces, ajoutez le IncludeTraces- : $false paramètre.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>L’emplacement de sortie par défaut sera le répertoire de \NCDiagnostics\ < working_directory >. Le répertoire de sortie par défaut peut être modifié à l’aide de le `-OutputDirectory` paramètre. 

Vous trouverez les informations d’état de Configuration SLB dans le _diagnostics-slbstateResults.Json_ fichier dans ce répertoire.

Ce fichier JSON peut être divisé selon les sections suivantes :
 * Infrastructure
   * SlbmVips - cette section répertorie l’adresse IP de l’adresse VIP de gestionnaire SLB qui est utilisé par le contrôleur de réseau à la configuration de coodinate et de contrôle d’intégrité entre les Agents hôtes de SLB MUX SLB.
   * MuxState - cette section répertorie une seule valeur pour chaque SLB/Mux déployée en donnant l’état de la mux
   * Configuration du routeur - cette section répertorie en amont du routeur (homologues BGP) numéro de système autonome (ASN), adresse IP de Transit et code. Il répertorie également le SLB MUX ASN et IP de Transit.
   * Connecté les informations de l’hôte - cette section liste l’adresse IP de gestion traitant tous les hôtes Hyper-V pour exécuter des charges de travail équilibrée.
   * Plages d’adresses IP virtuelles - cette section répertorie les plages de pools d’adresses IP virtuelles IP publiques et privées. Les adresses IP virtuelles SLBM sera inclus en tant qu’une adresse IP allouée à partir d’une de ces plages. 
   * Cette section répertorie MUX itinéraires - une seule valeur pour chaque SLB/Mux déployée contenant toutes les annonces d’itinéraire pour ce mux particulier.
 * Client
   * VipConsolidatedState - cette section répertorie l’état de connectivité pour chaque adresse IP virtuelle de locataire, y compris le préfixe d’itinéraire publié, hôte Hyper-V et les points de terminaison DIP.

> [!NOTE]
> État de SLB peuvent être vérifié directement à l’aide la [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script disponible sur le [dépôt Microsoft SDN GitHub](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validation de la passerelle

**Contrôleur de réseau :**
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

**À partir du commutateur Top of Rack (ToR) :**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Routeur BGP de Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Outre ces éléments, des problèmes que nous avons vus jusqu'à présent (surtout sur les déploiements en fonction de SDNExpress), la raison la plus courante pour le compartiment de locataire ne pas bien configuré sur des machines virtuelles GW semblent le fait que la capacité de GW dans FabricConfig.psd1 est inférieur par rapport à ce que Essayez d’assigner les connexions réseau (Tunnels S2S) dans TenantConfig.psd1 personnes. Cela peut être vérifiée facilement en comparant les sorties des commandes suivantes :

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Hébergeur] Valider le plan de données
Une fois que le contrôleur de réseau a été déployé, réseaux virtuels clients et les sous-réseaux ont été créés et machines virtuelles ont été attachées aux sous-réseaux virtuels, les tests de niveau de fabric supplémentaire peuvent être effectuées par l’hébergeur pour vérifier la connectivité du client.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Vérifiez la connectivité de réseau logique de fournisseur HNV
Après l’invité première machine virtuelle en cours d’exécution sur un ordinateur hôte Hyper-V a été connecté à un réseau virtuel locataire, le contrôleur de réseau affecte deux adresses IP de fournisseur HNV (adresses IP de fournisseur) à l’hôte Hyper-V. Ces adresses IP proviendront de Pool d’adresses IP du réseau logique de fournisseur HNV et être géré par le contrôleur de réseau.  Pour savoir quelles sont ces deux adresses IP de HNV du

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

Ces adresses IP du fournisseur HNV (PA IPs) sont affectés aux cartes Ethernet créé dans un compartiment de réseau TCP/IP distinct et ont un nom de l’adaptateur de _VLANX_ où X est le réseau local virtuel attribué au réseau logique de fournisseur HNV (transport).

Connectivité entre deux hôtes Hyper-V à l’aide du fournisseur HNV réseau logique peut être effectué par une commande ping avec un compartiment supplémentaire (-c Y) où Y est le compartiment de réseau TCP/IP dans lequel sont créés les PAhostVNICs de paramètre. Ce compartiment peut être déterminé en exécutant :

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
> Les adaptateurs de carte réseau virtuelle hôte de PA ne sont pas utilisés dans le chemin de données et par conséquent, vous n’ont pas d’une adresse IP affectée à la « carte vEthernet (PAhostVNic) ».

Par exemple, supposons qu’adresses IP de fournisseur HNV (PA) des hôtes Hyper-V 1 et 2 :

|Ordinateur hôte hyper-V-|-Adresse IP de PA 1|-Adresse IP : PA 2|
|---             |---            |---             |
|Hôte 1 | 10.10.182.64 | 10.10.182.65 |
|Hôte 2 | 10.10.182.66 | 10.10.182.67 |

Nous pouvons ping entre les deux à l’aide de la commande suivante pour vérifier la connectivité de réseau logique de fournisseur HNV.

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

*Mise à jour* ping de si fournisseur HNV ne fonctionne pas, vous vérifiez votre connectivité réseau physique, y compris la configuration de réseau local virtuel. Les cartes réseau physiques sur chaque hôte Hyper-V doivent être en mode trunk avec aucun réseau local virtuel spécifique attribué. La carte réseau virtuelle hôte de gestion doit être isolée sur un VLAN de réseau logique de gestion.

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

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Vérifier la prise en charge MTU et trame étendue sur le réseau logique de fournisseur HNV

Un autre problème courant dans le réseau logique de fournisseur HNV est que les ports de réseau physique et/ou de la carte Ethernet n’ont pas une taille MTU suffisamment grande configurée pour gérer la surcharge liée à partir de l’encapsulation VXLAN (ou NVGRE). 
>[!NOTE]
> Certaines cartes Ethernet et les pilotes prennent en charge la nouvelle * mot clé EncapOverhead qui sera automatiquement défini par l’Agent d’hôte de contrôleur de réseau à une valeur de 160. Cette valeur sera ensuite ajoutée à la valeur de la * mot clé JumboPacket dont la somme est utilisée en tant que la taille MTU publiée.
> par exemple, * EncapOverhead = 160 et * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Pour tester si le réseau logique de fournisseur HNV prend en charge la plus grande MTU taille end-to-end, utilisez le _Test-LogicalNetworkSupportsJumboPacket_ applet de commande :
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
* Ajuster la taille MTU sur les ports de commutateur physique au moins 1674B (y compris l’en-tête Ethernet de 14 et le code de fin)
* Si votre carte réseau ne prend pas en charge le mot clé EncapOverhead, ajustez le mot clé JumboPacket pour être au moins 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Vérifiez la connectivité de la carte réseau de machine virtuelle de locataire
Chaque carte réseau de machine virtuelle affectée à une machine virtuelle invitée possède un mappage de l’autorité de certification-PA entre le client adresse (autorité de certification privée) et l’espace d’adresse de fournisseur HNV (PA). Ces mappages sont conservées dans les tables du serveur OVSDB sur chaque hôte Hyper-V et sont accessibles en exécutant l’applet de commande suivante.

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
> Si les mappages de CA-PA voulus ne sont pas générés pour un client donné de machine virtuelle, consultez les ressources VM NIC et la Configuration IP sur le contrôleur de réseau à l’aide de la _Get-NetworkControllerNetworkInterface_ applet de commande. En outre, vérifiez les connexions établies entre les nœuds de contrôleur de réseau hôte Agent et le contrôleur de réseau.

Avec ces informations, un ping de la machine virtuelle locataire peut maintenant être lancée par l’hébergeur du contrôleur de réseau à l’aide de la _Test-VirtualNetworkConnection_ applet de commande.

## <a name="specific-troubleshooting-scenarios"></a>Scénarios de résolution des problèmes spécifiques

Les sections suivantes fournissent des conseils pour résoudre des scénarios spécifiques.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Aucune connectivité réseau entre les deux machines virtuelles de locataire

1.  [Tenant] Vérifiez le que pare-feu Windows dans les machines virtuelles du locataire ne bloque pas le trafic.  
2.  [Tenant] Vérifiez que les adresses IP affectées à la machine virtuelle locataire en exécutant _ipconfig_. 
3.  [Hébergeur] Exécutez **Test-VirtualNetworkConnection** à partir de l’hôte Hyper-V pour valider la connectivité entre les machines virtuelles deux client en question. 

>[!NOTE]
>Le VSID fait référence à l’ID de sous-réseau virtuel. Dans le cas VXLAN, il s’agit de l’identificateur de réseau VXLAN (VNI). Vous pouvez trouver cette valeur en exécutant la **Get-PACAMapping** applet de commande.

#### <a name="example"></a>Exemple

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Créer des « sa18n30-2.sa18.nttest.microsoft.com » avec IP Mgmt de 10.127.132.153 à ListenerCA IP de 192.168.1.5 que toutes deux liées au sous-réseau virtuel (VSID) 4114 autorité de certification-ping entre « Web vert machine virtuelle 1 » avec IP SenderCA de 192.168.1.4 sur l’hôte.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Test ping d’espace de l’autorité de certification à compter à partir de la session de trace Ping à 192.168.1.5 a réussi à partir de l’adresse 192.168.1.4 Rtt = 0 ms


Informations de routage autorité de certification :

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

 <snip> ...

4. [Tenant] Vérifiez qu’il n’existe aucune stratégie de pare-feu distribué spécifié sur le sous-réseau virtuel ou les interfaces de réseau de machine virtuelle qui bloqueraient le trafic.    

Interroger l’API REST de contrôleur de réseau trouvé dans l’environnement de démonstration à sa18n30nc dans le domaine sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Regardez à la Configuration IP et sous-réseaux virtuels qui font référence à cette liste ACL

1. [Hébergeur] Exécutez ``Get-ProviderAddress`` sur les deux Hyper-V qui héberge les deux hôtes de machines virtuelles dans la question du locataire, puis exécutez ``Test-LogicalNetworkConnection`` ou ``ping -c <compartment>`` à partir de l’hôte Hyper-V pour valider la connectivité sur le réseau logique de fournisseur HNV
2.  [Hébergeur] Assurez-vous que les paramètres MTU sont correctes sur les ordinateurs hôtes Hyper-V et toute couche 2 périphériques entre les hôtes Hyper-V de commutation. Exécutez ``Test-EncapOverheadValue`` sur tous les hôtes Hyper-V en question. Vérifiez également que tous les commutateurs de couche 2 entre les deux ont MTU définir octets 1674 minimum pour prendre en compte la charge maximale d’octets de 160.  
3.  [Hébergeur] Si des adresses IP PA ne sont pas présents et/ou de connectivité de l’autorité de certification est interrompue, vérifiez la stratégie de réseau a été reçue. Exécutez ``Get-PACAMapping`` pour voir si les règles de l’encapsulation et les mappages de CA-PA requis pour la création de réseaux virtuels de superposition sont correctement établies.  
4.  [Hébergeur] Vérifiez que l’Agent hôte du contrôleur de réseau est connecté au contrôleur de réseau. Exécutez ``netstat -anp tcp |findstr 6640`` pour voir si le   
5.  [Hébergeur] Vérifiez que l’ID de l’hôte dans HKLM / correspond à l’ID d’Instance des ressources du serveur qui héberge les machines virtuelles de locataire.  
6. [Hébergeur] Vérifiez que l’ID de profil de Port correspond à l’ID d’Instance des Interfaces de réseau de machine virtuelle des ordinateurs virtuels client.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Journalisation, suivi et les diagnostics avancés

Les sections suivantes fournissent des informations sur les diagnostics avancés, journalisation et de suivi.

### <a name="network-controller-centralized-logging"></a>Contrôleur de réseau centralisée de journalisation 

Le contrôleur de réseau peut automatiquement collecter les journaux de débogueur et les stocker dans un emplacement centralisé. Collecte de journaux peut être activée lorsque vous déployez le contrôleur de réseau pour la première fois ou à tout moment ultérieurement. Les journaux sont collectés à partir du contrôleur de réseau et les éléments gérés par le contrôleur de réseau du réseau : héberger des machines, les équilibreurs de charge logiciel (SLB) et les ordinateurs de passerelle. 

Ces journaux incluent les journaux de débogage pour le cluster de contrôleur de réseau, l’application de contrôleur de réseau, les journaux de passerelle, SLB, un réseau virtuel et le pare-feu distribué. Chaque fois qu’une nouvelle hôte/passerelle/SLB est ajoutée au contrôleur de réseau, la journalisation est démarrée sur ces ordinateurs. De même, lorsqu’une hôte/de SLB/la passerelle est supprimée à partir du contrôleur de réseau, la connexion est arrêtée sur ces ordinateurs.

#### <a name="enable-logging"></a>Activer la journalisation

La journalisation est activée automatiquement lorsque vous installez le cluster de contrôleur de réseau à l’aide de la **Install-NetworkControllerCluster** applet de commande. Par défaut, les journaux sont collectés localement sur les nœuds de contrôleur de réseau à *%systemdrive%\SDNDiagnostics*. Il est **fortement recommandé** modifier cet emplacement pour être un partage de fichiers distant (non local). 

Les journaux de cluster de contrôleur de réseau sont stockés à *%programData%\Windows Fabric\log\Traces*. Vous pouvez spécifier un emplacement centralisé pour la collecte de journaux avec le **DiagnosticLogLocation** paramètre à la recommandation qu’il s’agit également être un partage de fichiers distant. 

Si vous souhaitez restreindre l’accès à cet emplacement, vous pouvez fournir les informations d’identification d’accès avec le **LogLocationCredential** paramètre. Si vous fournissez les informations d’identification pour accéder à l’emplacement du journal, vous devez également fournir le **CredentialEncryptionCertificate** paramètre, qui est utilisé pour chiffrer les informations d’identification stockées localement sur les nœuds de contrôleur de réseau.  

Les paramètres par défaut, il est recommandé d’avoir au moins 75 Go d’espace libre dans l’emplacement central et 25 Go sur les nœuds locales (si vous N'utilisez pas un emplacement central) pour un cluster de contrôleur de réseau 3 nœuds.

#### <a name="change-logging-settings"></a>Modifier les paramètres de journalisation

Vous pouvez modifier les paramètres de journalisation à tout moment le ``Set-NetworkControllerDiagnostic`` applet de commande. Les paramètres suivants peuvent être modifiés :

- **Emplacement du journal centralisé**.  Vous pouvez modifier l’emplacement où stocker les journaux, avec le ``DiagnosticLogLocation`` paramètre.  
- **Informations d’identification pour accéder à l’emplacement du journal**.  Vous pouvez modifier les informations d’identification pour accéder à l’emplacement du journal, avec le ``LogLocationCredential`` paramètre.  
- **Déplacer vers la journalisation locale**.  Si vous avez fourni un emplacement centralisé pour stocker les journaux, vous pouvez revenir à la journalisation localement sur les nœuds de contrôleur de réseau avec la ``UseLocalLogLocation`` paramètre (non recommandé en raison des exigences d’espace disque volumineux).  
- **Journalisation étendue**.  Par défaut, tous les journaux sont collectés. Vous pouvez modifier l’étendue pour collecter des journaux de cluster de contrôleur de réseau uniquement.  
- **Niveau de journalisation**.  Le niveau de journalisation par défaut est information. Vous pouvez le modifier à l’erreur, avertissement ou Verbose.  
- **Journal vieillissement temps**.  Les journaux sont stockés de manière circulaire. Vous avez 3 jours de données de journalisation par défaut, que vous utilisiez la journalisation locale ou une connexion centralisée. Vous pouvez modifier cette limite de temps avec **LogTimeLimitInDays** paramètre.  
- **Vieillissement de taille de journal**.  Par défaut, vous devez un maximale 75 Go de données de journalisation à l’aide de la journalisation centralisée et 25 Go si vous utilisez la journalisation locale. Vous pouvez modifier cette limite avec la **LogSizeLimitInMBs** paramètre.

#### <a name="collecting-logs-and-traces"></a>Traces et collecte des journaux

Déploiements VMM utilisent une connexion centralisée pour le contrôleur de réseau par défaut. L’emplacement du partage de fichier pour ces journaux est spécifié lors du déploiement du modèle de service de contrôleur de réseau.

Si un emplacement de fichier n’a pas été spécifié, local journalisation servira sur chaque nœud du contrôleur de réseau avec les journaux enregistrés sous C:\Windows\tracing\SDNDiagnostics. Ces journaux sont enregistrés à l’aide de la hiérarchie suivante :

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Traces

Le contrôleur de réseau utilise (Azure) Service Fabric. Journaux service Fabric peuvent être nécessaire lors du dépannage de certains problèmes. Vous trouverez ces journaux sur chaque nœud de contrôleur de réseau à C:\ProgramData\Microsoft\Service Fabric.

Si un utilisateur a exécuté le _NetworkController de débogage_ applet de commande, des journaux supplémentaires seront disponibles sur chaque hôte Hyper-V qui a été spécifié avec une ressource de serveur dans le contrôleur de réseau. Ces journaux (et des traces si activé) sont conservées sous C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnostics SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erreurs d’infrastructure de SLBM (actions de fournisseur d’hébergement service)

1.  Vérifiez que Manager d’équilibreur de charge logicielle (SLBM) fonctionne et que les couches d’orchestration peuvent communiquer entre eux : SLBM -> SLB/Mux et SLBM -> Agents hôtes SLB. Exécutez [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) à partir de n’importe quel nœud avec un accès au point de terminaison REST de contrôleur de réseau.  
2.  Valider la *SDNSLBMPerfCounters* dans PerfMon sur le nœud de contrôleur de réseau des machines virtuelles (de préférence le contrôleur de réseau nœud principal - Get-NetworkControllerReplica) :
    1.  Moteur de Load Balancer (LB) est connecté à SLBM ? (*SLBM LBEngine Configurations Total* > 0)  
    2.  SLBM sait-il au moins sur ses propres points de terminaison ? (*Total de points de terminaison VIP* > = 2)  
    3.  Ordinateurs hôtes Hyper-V (DIP) sont connectés à SLBM ? (*Clients HP connectés* == num serveurs)   
    4.  SLBM est connecté à multiplexeurs ? (*Multiplexeurs connecté* == *multiplexeurs intègre sur le SLBM* == *multiplexeurs signalés comme étant intègres* = # des machines virtuelles MUX SLB).  
3.  Vérifiez le routeur BGP configuré est l’homologation avec succès avec le SLB MUX  
    1.  Si vous utilisez RRAS avec accès à distance (par exemple, BGP virtual machine) :  
        1.  Get-paire BGP a doit afficher connecté  
        2.  Get-BgpRouteInformation doit afficher au moins un itinéraire pour le SLBM self VIP  
    2.  Si à l’aide de la physique Top-of-Rack (ToR) Basculer en tant qu’homologue BGP, consultez la documentation  
        1.  Par exemple : # afficher l’instance de bgp  
4.  Valider la *SlbMuxPerfCounters* et *SLBMUX* compteurs PerfMon sur la machine virtuelle SLB Mux
5.  Vérifier l’état de configuration et les plages d’adresses IP virtuelles dans les ressources du Gestionnaire d’équilibrage de charge logiciel  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| convertto-json-profondeur 8 (Vérifiez les plages d’adresses IP virtuelles dans des Pools d’adresses IP et assurez-vous que self-adresses IP virtuelles SLBM (*LoadBalanacerManagerIPAddress*) et n’importe quel adresses IP virtuelles côté locataire figurent dans ces plages d’adresses)  
        1. Get-NetworkControllerIpPool - ID du réseau « < publique/privée VIP logique réseau Resource ID > » - SubnetId « < publique/privée VIP logique Resource ID de sous-réseau > » ResourceId-«<IP Pool Resource Id>» - ConnectionUri $uri | convertto-json-profondeur 8 
    2.  Debug-NetworkControllerConfigurationState -  

Si des vérifications ci-dessus échouent, l’état SLB locataire sera également dans un mode d’échec.  

**Mise à jour**   
Selon les informations de diagnostics suivantes présentées, les erreurs suivantes :  
* Vérifiez que les multiplexeurs SLB sont connectés.  
  * Résoudre les problèmes de certificat  
  * Résoudre les problèmes de connectivité réseau  
* Vérifiez les informations sur l’homologation BGP sont configurées correctement  
* Garantir l’ID d’Instance de serveur dans la ressource serveur correspond à l’ID d’hôte dans le Registre (référence annexe pour *HostNotConnected* code d’erreur)  
* Collecter les journaux  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erreurs SLBM locataire (hébergement fournisseur et client actions de service)

1.  [Hébergeur] Vérifiez *NetworkControllerConfigurationState de débogage* pour voir si des ressources de l’équilibreur de charge sont dans un état d’erreur. Essayez de réduire en suivant les éléments d’Action Table dans l’annexe.   
    1.  Vérifiez qu’un point de terminaison VIP est présent et les itinéraires croisés  
    2.  Vérifiez les points de terminaison DIP combien ont été détectés pour le point de terminaison VIP  
2.  [Tenant] Valider les ressources Load Balancer sont correctement spécifiées.  
    1.  Valider l’adresse DIP points de terminaison qui sont inscrits dans SLBM hébergent des machines virtuelles du locataire qui correspondent à des configurations IP LoadBalancer Back-end adresse pool  
3.  [Hébergeur] Si le point de terminaison DIP n’est pas découverts ou connecté :   
    1.  Vérifiez *NetworkControllerConfigurationState de débogage*  
        1.  Valider ce contrôleur de réseau et l’Agent hôte de SLB est correctement connecté par le coordinateur d’événements de contrôleur de réseau ``netstat -anp tcp |findstr 6640)``  
    2.  Vérifiez *HostId* dans *nchostagent* service regkey (référence *HostNotConnected* code d’erreur dans l’annexe) correspond à l’instance de la ressource de serveur correspondant Id () ``Get-NCServer |convertto-json -depth 8``)  
    3.  Vérifiez l’Id d’Instance correspondant machine virtuelle carte réseau de la ressource correspond à l’id de profil de port pour le port de machine virtuelle   
4.  [Fournisseur d’hébergement] Collecter les journaux   

#### <a name="slb-mux-tracing"></a>Suivi de SLB Mux

Pour plus d’informations à partir de multiplexeurs d’équilibrage de charge logiciel peuvent également être déterminés grâce Observateur d’événements. 
1. Cliquez sur « Afficher et déboguer les journaux d’analyse » dans le menu Affichage de l’Observateur d’événement
2. Accédez à « Journaux des Applications et Services » > Microsoft > Windows > SlbMuxDriver > dans l’Observateur d’événements de Trace 
3. Cliquez avec le bouton droit dessus et sélectionnez « Activer le journal »

>[!NOTE]
>Il est recommandé que vous avez uniquement cette journalisation activée pendant une courte période lorsque vous essayez de reproduire un problème

### <a name="vfp-and-vswitch-tracing"></a>VFP et suivi du commutateur virtuel

À partir de n’importe quel Hyper-V hôte qui héberge un ordinateur virtuel attaché à un réseau virtuel locataire invité, vous pouvez collectées une trace VFP pour déterminer où potentielle des problèmes.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```

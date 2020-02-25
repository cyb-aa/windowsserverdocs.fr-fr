---
title: Choix d’une carte réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9271cf4e5f50adf93f421e830a226507034ac454
ms.sourcegitcommit: 1c75e4b3f5895f9fa33efffd06822dca301d4835
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77517474"
---
# <a name="choosing-a-network-adapter"></a>Choix d’une carte réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour découvrir certaines des fonctionnalités des cartes réseau qui peuvent affecter vos choix d’achat.

Les applications nécessitant beaucoup de ressources réseau requièrent des cartes réseau hautes performances. Cette section explore certains points à prendre en compte pour le choix des cartes réseau, ainsi que la configuration de différents paramètres de carte réseau pour obtenir les meilleures performances réseau.

> [!TIP]
>  Vous pouvez configurer les paramètres de carte réseau à l’aide de Windows PowerShell. Pour plus d’informations, consultez [applets de commande de cartes réseau dans Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter).

##  <a name="bkmk_offload"></a>Fonctionnalités de déchargement

Le déchargement des tâches à partir de l’unité centrale \(processeur\) à la carte réseau peut réduire l’utilisation du processeur sur le serveur, ce qui améliore les performances globales du système.

La pile réseau des produits Microsoft peut décharger une ou plusieurs tâches sur une carte réseau si vous sélectionnez une carte réseau qui dispose des fonctionnalités de déchargement appropriées. Le tableau suivant fournit une brève présentation des différentes fonctionnalités de déchargement disponibles dans Windows Server 2016.
  
|Type de déchargement|Description|
|------------------|-----------------|  
|Calcul de la somme de contrôle pour TCP|La pile réseau peut décharger le calcul et la validation du protocole de contrôle de transmission \(les checksums TCP\) sur les chemins de code d’envoi et de réception. Il peut également décharger le calcul et la validation des sommes de contrôle IPv4 et IPv6 sur les chemins de code d’envoi et de réception.|  
|Calcul de la somme de contrôle pour UDP |La pile réseau peut décharger le calcul et la validation du protocole UDP (User Datagram Protocol) \(les sommes de contrôle de\) UDP sur les chemins de code d’envoi et de réception.|
|Calcul de la somme de contrôle pour IPv4 |La pile réseau peut décharger le calcul et la validation des sommes de contrôle IPv4 sur les chemins d’accès de code d’envoi et de réception. |
|Calcul de la somme de contrôle pour IPv6 |La pile réseau peut décharger le calcul et la validation des sommes de contrôle IPv6 sur les chemins d’accès de code d’envoi et de réception. | 
|Segmentation de paquets TCP volumineux|La couche de transport TCP/IP prend en charge le déchargement d’envoi important v2 (LSOv2). Avec LSOv2, la couche de transport TCP/IP peut décharger la segmentation des paquets TCP volumineux vers la carte réseau.|  
|Mise à l’échelle côté réception \(RSS\)|RSS est une technologie de pilote réseau qui permet de distribuer efficacement le traitement de réception réseau sur plusieurs processeurs dans des systèmes multiprocesseurs. Plus d’informations sur RSS sont fournies plus loin dans cette rubrique.|  
|Réception de la fusion de segment \(RSC\)|RSC est la possibilité de regrouper les paquets pour réduire le traitement d’en-tête nécessaire à l’exécution de l’hôte. Un maximum de 64 Ko de charge utile reçue peut être fusionné en un seul paquet plus grand à traiter. Plus de détails sur RSC sont fournis plus loin dans cette rubrique.|  
  
###  <a name="bkmk_rss"></a>Mise à l’échelle côté réception

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 et Windows Server 2008 prennent en charge la mise à l’échelle côté réception \(les\)RSS. 

Certains serveurs sont configurés avec plusieurs processeurs logiques qui partagent des ressources matérielles \(tels qu’un\) de base physique et qui sont traités comme des homologues multithread \(SMT\) simultanés. La technologie Intel Hyper-Threading est un exemple. RSS dirige le traitement réseau jusqu’à un seul processeur logique par cœur. Par exemple, sur un serveur doté de l’Hyper-Threading Intel, de 4 cœurs et de 8 processeurs logiques, RSS n’utilise pas plus de 4 processeurs logiques pour le traitement réseau.  

RSS distribue les paquets d’e/s réseau entrants entre les processeurs logiques afin que les paquets appartenant à la même connexion TCP soient traités sur le même processeur logique, ce qui préserve le classement. 

RSS équilibre également la charge du trafic de monodiffusion et de monodiffusion UDP, et achemine les flux associés \(qui sont déterminés par le hachage des adresses source et de destination\) au même processeur logique, en préservant l’ordre des arrivées associées. Cela permet d’améliorer l’évolutivité et les performances des scénarios nécessitant beaucoup de ressources de réception pour les serveurs qui ont moins de cartes réseau que les processeurs logiques éligibles. 

#### <a name="configuring-rss"></a>Configuration de RSS

Dans Windows Server 2016, vous pouvez configurer RSS à l’aide des applets de commande Windows PowerShell et des profils RSS. 

Vous pouvez définir des profils RSS à l’aide du paramètre **– Profile** de l’applet de commande Windows PowerShell **Set-NetAdapterRss** .

**Commandes Windows PowerShell pour la configuration RSS**

Les applets de commande suivantes vous permettent de voir et de modifier les paramètres RSS par carte réseau.
  
>[!NOTE]
>Pour obtenir une référence de commande détaillée pour chaque cmdlet, y compris la syntaxe et les paramètres, vous pouvez cliquer sur les liens suivants. En outre, vous pouvez passer le nom de l’applet de commande à l’aide de l’invite de commandes Windows PowerShell pour **obtenir** des détails sur chaque commande.  

- [Disable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Disable-NetAdapterRss). Cette commande désactive RSS sur la carte réseau que vous spécifiez.

- [Enable-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterRss). Cette commande active RSS sur la carte réseau que vous spécifiez.
  
- [Accédez à NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Get-NetAdapterRss). Cette commande récupère les propriétés RSS de la carte réseau que vous spécifiez.
  
- [Set-NetAdapterRss](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterRss). Cette commande définit les propriétés RSS sur la carte réseau que vous spécifiez.  

#### <a name="rss-profiles"></a>Profils RSS

Vous pouvez utiliser le paramètre **– Profile** de l’applet de commande Set-NetAdapterRss pour spécifier les processeurs logiques qui sont affectés à la carte réseau. Les valeurs disponibles pour ce paramètre sont les suivantes :

- **Le plus proche**. Les nombres de processeurs logiques qui sont proches du processeur RSS de base de la carte réseau sont préférés. Avec ce profil, le système d’exploitation peut rééquilibrer dynamiquement les processeurs logiques en fonction de la charge.
  
- **ClosestStatic**. Les nombres de processeurs logiques proches du processeur RSS de base de la carte réseau sont préférés. Avec ce profil, le système d’exploitation ne rééquilibre pas dynamiquement les processeurs logiques en fonction de la charge.
  
- **NUMA**. Les numéros de processeur logique sont généralement sélectionnés sur différents nœuds NUMA pour distribuer la charge. Avec ce profil, le système d’exploitation peut rééquilibrer dynamiquement les processeurs logiques en fonction de la charge.
  
- **NUMAStatic**. Il s’agit du **profil par défaut**. Les numéros de processeur logique sont généralement sélectionnés sur différents nœuds NUMA pour distribuer la charge. Avec ce profil, le système d’exploitation ne rééquilibrera pas de manière dynamique les processeurs logiques en fonction de la charge.

- **Conservateur**. RSS utilise le moins de processeurs possible pour supporter la charge. Cette option permet de réduire le nombre d’interruptions.

Selon le scénario et les caractéristiques de la charge de travail, vous pouvez également utiliser d’autres paramètres de l’applet de commande Windows PowerShell **Set-NetAdapterRss** pour spécifier les éléments suivants :

- Sur la base d’une carte réseau, combien de processeurs logiques peuvent être utilisés pour RSS.
- Décalage de début de la plage de processeurs logiques.
- Nœud à partir duquel la carte réseau alloue de la mémoire.

Voici les paramètres **Set-NetAdapterRss** supplémentaires que vous pouvez utiliser pour configurer RSS :

>[!NOTE]
>Dans l’exemple de syntaxe pour chaque paramètre ci-dessous, le nom de la carte réseau **Ethernet** est utilisé comme exemple de valeur pour le paramètre **– Name** de la commande **Set-NetAdapterRss** . Lorsque vous exécutez l’applet de commande, assurez-vous que le nom de la carte réseau que vous utilisez est adapté à votre environnement.

- **\* MaxProcessors**: définit le nombre maximal de processeurs RSS à utiliser. Cela garantit que le trafic de l’application est lié à un nombre maximal de processeurs sur une interface donnée. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**: définit le groupe de processeurs de base d’un nœud NUMA. Cela a un impact sur le groupe de processeurs utilisé par RSS. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: définit le groupe de processeurs Max d’un nœud NUMA. Cela a un impact sur le groupe de processeurs utilisé par RSS. Cela limiterait un groupe de processeurs maximal pour que l’équilibrage de charge soit aligné dans un groupe k. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: définit le numéro de processeur de base d’un nœud NUMA. Cela a un impact sur le groupe de processeurs utilisé par RSS. Cela permet de partitionner les processeurs entre les cartes réseau. Il s’agit du premier processeur logique de la plage de processeurs RSS qui est affectée à chaque carte. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: nœud NUMA à partir duquel chaque carte réseau peut allouer de la mémoire. Cela peut se faire dans un groupe k ou dans différents groupes k. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: si vos processeurs logiques semblent être sous-utilisés pour la réception du trafic \(par exemple, comme affiché dans le gestionnaire des tâches\), vous pouvez essayer d’agrandir le nombre de files d’attente RSS de la valeur par défaut de 2 à la valeur maximale prise en charge par votre carte réseau. Votre carte réseau peut avoir des options permettant de modifier le nombre de files d’attente RSS dans le cadre du pilote. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Pour plus d’informations, cliquez sur le lien suivant pour télécharger [la mise en réseau évolutive : élimination du goulot d’étranglement du traitement des réceptions, présentation de RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) au format Word.
  
#### <a name="understanding-rss-performance"></a>Fonctionnement des performances RSS

Le paramétrage de RSS requiert la compréhension de la configuration et de la logique d’équilibrage de charge. Pour vérifier que les paramètres RSS ont pris effet, vous pouvez passer en revue la sortie quand vous exécutez l’applet de commande Windows PowerShell **NetAdapterRss** . Voici un exemple de sortie de cette applet de commande.
  
```

PS C:\Users\Administrator> get-netadapterrss  
Name                           : testnic 2  
InterfaceDescription           : Broadcom BCM5708C NetXtreme II GigE (NDIS VBD Client) #66
Enabled                        : True
NumberOfReceiveQueues          : 2
Profile                        : NUMAStatic
BaseProcessor: [Group:Number]  : 0:0
MaxProcessor: [Group:Number]   : 0:15
MaxProcessors                  : 8
  
IndirectionTable: [Group:Number]:
     0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
…   
(# indirection table entries are a power of 2 and based on # of processors)  
…   
                          0:0    0:4    0:0    0:4    0:0    0:4    0:0    0:4  
```  

En plus de répercuter les paramètres qui ont été définis, l’aspect clé de la sortie est la sortie de la table d’indirection. La table d’indirection affiche les compartiments de table de hachage utilisés pour distribuer le trafic entrant. Dans cet exemple, la notation n :c désigne la paire NUMA K-Group : CPU index qui est utilisée pour diriger le trafic entrant. Nous voyons exactement 2 entrées uniques (0:0 et 0:4), qui représentent respectivement k-Group 0/CPU0 et k-Group 0/CPU 4.

Il n’existe qu’un seul groupe k pour ce système (k-Group 0) et une entrée de table n (où n < = 128). Étant donné que le nombre de files d’attente de réception est défini sur 2, seuls 2 processeurs (0:0, 0:4) sont choisis, même si le nombre maximal de processeurs est défini sur 8. En effet, la table d’indirection effectue le hachage du trafic entrant pour n’utiliser que 2 processeurs du 8 qui sont disponibles.

Pour utiliser pleinement les processeurs, le nombre de files d’attente de réception RSS doit être supérieur ou égal au nombre maximal de processeurs. Dans l’exemple précédent, la file d’attente de réception doit avoir une valeur supérieure ou égale à 8.

#### <a name="nic-teaming-and-rss"></a>Association de cartes réseau et RSS

RSS peut être activé sur une carte réseau qui est associée à une autre carte d’interface réseau à l’aide de l’Association de cartes réseau. Dans ce scénario, seule la carte réseau physique sous-jacente peut être configurée pour utiliser RSS. Un utilisateur ne peut pas définir des applets de commande RSS sur la carte réseau associée.
  
###  <a name="bkmk_rsc"></a>Fusion de segment de réception (RSC)

La fusion de segment de réception \(RSC\) permet d’obtenir des performances en réduisant le nombre d’en-têtes IP traités pour une quantité donnée de données reçues. Elle doit être utilisée pour aider à mettre à l’échelle les performances des données reçues en regroupant \(ou en fusionnant\) les plus petits paquets en unités plus volumineuses.

Cette approche peut avoir une incidence sur la latence avec les avantages généralement constatés dans les gains de débit. RSC est recommandé pour augmenter le débit des charges de travail lourdes reçues. Envisagez de déployer des cartes réseau qui prennent en charge RSC. 

Sur ces cartes réseau, assurez-vous que RSC est activé \(il s’agit du paramètre par défaut\), sauf si vous avez des charges de travail spécifiques \(par exemple, une faible latence, une mise en réseau à faible débit\) qui montre l’avantage de RSC en cours de désactivation.

#### <a name="understanding-rsc-diagnostics"></a>Fonctionnement des diagnostics RSC

Vous pouvez diagnostiquer RSC à l’aide des applets de commande Windows PowerShell **NetAdapterRsc** et de la commande **NetAdapterStatistics**.

Voici un exemple de sortie lorsque vous exécutez l’applet de commande NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

L’applet de commande d' **obtention** indique si RSC est activé dans l’interface et si le protocole TCP permet à RSC d’être dans un état opérationnel. La raison de l’échec fournit des détails sur l’échec de l’activation de RSC sur cette interface.

Dans le scénario précédent, IPv4 RSC est pris en charge et opérationnel dans l’interface. Pour comprendre les échecs de diagnostic, vous pouvez voir les octets fusionnés ou les exceptions déclenchées. Cela fournit une indication des problèmes de fusion.

Voici un exemple de sortie lorsque vous exécutez l’applet de commande NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC et virtualisation

RSC est pris en charge uniquement dans l’hôte physique lorsque la carte réseau de l’ordinateur hôte n’est pas liée au commutateur virtuel Hyper-V. RSC est désactivé par le système d’exploitation lorsque l’ordinateur hôte est lié au commutateur virtuel Hyper-V. En outre, les machines virtuelles n’obtiennent pas l’avantage de RSC, car les cartes réseau virtuelles ne prennent pas en charge RSC.

RSC peut être activé pour un ordinateur virtuel lorsque la virtualisation d’entrée/sortie de racine unique \(SR-IOV\) est activée. Dans ce cas, les fonctions virtuelles prennent en charge la fonction RSC ; par conséquent, les machines virtuelles bénéficient également de l’avantage de RSC.

##  <a name="bkmk_resources"></a>Ressources de la carte réseau

Quelques cartes réseau gèrent activement leurs ressources pour obtenir des performances optimales. Plusieurs cartes réseau vous permettent de configurer manuellement des ressources à l’aide de l’onglet **mise en réseau avancée** pour la carte. Pour ces adaptateurs, vous pouvez définir les valeurs d’un certain nombre de paramètres, notamment le nombre de tampons de réception et de mémoires tampons d’envoi.

La configuration des ressources de carte réseau est simplifiée par l’utilisation des applets de commande Windows PowerShell suivantes.

- [NetAdapterAdvancedProperty](https://docs.microsoft.com/powershell/module/netadapter/Get-NetAdapterAdvancedProperty)

- [Set-NetAdapterAdvancedProperty](https://docs.microsoft.com/powershell/module/netadapter/Set-NetAdapterAdvancedProperty)

- [Activer-NetAdapter](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapte)

- [Enable-NetAdapterBinding](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterBinding)

- [Enable-NetAdapterChecksumOffload](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Enable-NetAdapterIPSecOffload](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterChecksumOffload)

- [Enable-NetAdapterLso](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterLso)

- [Enable-NetAdapterPowerManagement](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterPowerManagement)

- [Enable-NetAdapterQos](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterQos)

- [Enable-NetAdapterRDMA](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterRDMA)

- [Enable-NetAdapterSriov](https://docs.microsoft.com/powershell/module/netadapter/Enable-NetAdapterSriov)

Pour plus d’informations, consultez [applets de commande de cartes réseau dans Windows PowerShell](https://docs.microsoft.com/powershell/module/netadapter).

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).

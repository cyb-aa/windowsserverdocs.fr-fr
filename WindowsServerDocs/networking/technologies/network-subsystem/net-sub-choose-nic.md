---
title: Choix d’une carte réseau
description: Cette rubrique fait partie du guide de réglage des performances sous-système réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 4b3b9d206273dfd0e9115ebc27cf28aa960bfb0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="choosing-a-network-adapter"></a>Choix d’une carte réseau

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette rubrique pour découvrir certaines des fonctionnalités des cartes réseau susceptibles d’affecter vos choix d’achat.

Les applications gourmandes en réseau nécessitent des cartes réseau à hautes performances. Cette section présente certaines considérations sur la sélection des cartes réseau, ainsi que comment configurer les paramètres de la carte réseau différente pour optimiser les performances réseau.

> [!TIP]
>  Vous pouvez configurer les paramètres de carte réseau à l’aide de Windows PowerShell. Pour plus d’informations, voir [les applets de commande de carte réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a>Fonctions de déchargement

Déchargeant des tâches de l’unité centrale \(CPU\) à la carte réseau peut réduire l’utilisation du processeur sur le serveur, ce qui améliore les performances globales du système.

La pile réseau dans les produits Microsoft peut décharger une ou plusieurs tâches à une carte réseau si vous sélectionnez une carte réseau qui a le déchargement de fonctionnalités. Le tableau suivant fournit une vue d’ensemble de capacités de déchargement différents qui sont disponibles dans Windows Server 2016.
  
|Type de déchargement|Description|
|------------------|-----------------|  
|Calcul de somme de contrôle TCP|La pile réseau peut décharger le calcul et la validation des sommes de contrôle de Transmission Control Protocol \(TCP\) sur Envoyer et recevoir des chemins de code. Il peut également décharger le calcul et la validation de IPv4 et IPv6 des sommes de contrôle sur envoient et recevoir des chemins de code.|  
|Calcul de somme de contrôle pour le protocole UDP |La pile réseau peut décharger le calcul et la validation des sommes de contrôle \(UDP\) User Datagram Protocol sur Envoyer et recevoir des chemins de code.|
|Calcul de somme de contrôle pour IPv4 |La pile réseau peut décharger le calcul et la validation de IPv4 sommes de contrôle sur envoient et recevoir des chemins de code. |
|Calcul de somme de contrôle pour IPv6 |La pile réseau peut décharger le calcul et la validation du protocole IPv6 une somme de contrôle sur Envoyer et recevoir des chemins de code. | 
|Segmentation des paquets TCP volumineux|Prend en charge de la couche de transport TCP/IP v2 déchargement d’envoi important (LSOv2). Avec LSOv2, la couche de transport TCP/IP peut décharger la segmentation des paquets TCP volumineux à la carte réseau.|  
|Trafic entrant \(RSS\)|RSS est une technologie de pilote réseau qui permet la distribution efficace du réseau de traitement de réception sur plusieurs processeurs dans des systèmes multiprocesseurs. Vous trouverez plus d’informations sur RSS plus loin dans cette rubrique.|  
|Recevoir le Segment de fusion \(RSC\)|RSC est la possibilité de paquets de groupe afin de réduire l’en-tête de traitement qui est nécessaire pour l’ordinateur hôte à effectuer. Un maximum de 64 Ko de la charge utile de reçus permettre être fusionné dans une taille de paquet unique pour le traitement. Plus de détails sur RSC est fournie plus loin dans cette rubrique.|  
  
###  <a name="bkmk_rss"></a>Trafic entrant

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 et Windows Server 2008 prennent en charge \(RSS\) l’échelle côté réception. 

Certains serveurs sont configurés avec plusieurs processeurs logiques qui partagent les ressources matérielles \ (par exemple, un core\ physique) et qui sont traité comme \(SMT\) multithreading simultanées homologues. La technologie Intel Hyper-Threading est un exemple. RSS dirige jusqu'à un processeur logique par cœur de traitement réseau. Par exemple, sur un serveur avec Hyper-Threading Intel, 8 processeurs logiques et 4 cœurs, RSS utilise pas plus de 4 processeurs logiques pour le traitement du réseau.  

RSS distribue les paquets entrants des e/s réseau entre les processeurs logiques afin que les paquets qui appartiennent à la même connexion TCP sont traités sur le même processeur logique qui conserve l’ordre. 

RSS également charger les soldes UDP monodiffusion et le trafic de multidiffusion, et il achemine les flux connexes \ (qui sont déterminé par le hachage de la source et destination addresses\) avec le même processeur logique, en conservant l’ordre des applications connexes. Cela permet d’améliorer l’évolutivité et les performances pour les scénarios intensives côté réception pour les serveurs qui ont moins de cartes réseau qu’ils éligibles processeurs logiques. 

#### <a name="configuring-rss"></a>Configuration de RSS

Dans Windows Server 2016, vous pouvez configurer RSS à l’aide des applets de commande Windows PowerShell et les profils RSS. 

Vous pouvez définir les profils RSS à l’aide de la **– profil** paramètre de la **Set-NetAdapterRss** applet de commande Windows PowerShell.

**Commandes Windows PowerShell pour la configuration de RSS**

Les applets de commande suivantes permettent d’afficher et modifier les paramètres RSS par la carte réseau.
  
>[!NOTE]
>Pour des informations détaillées de commande pour chaque applet de commande, y compris la syntaxe et les paramètres, vous pouvez cliquer sur les liens suivants. En outre, vous pouvez transmettre le nom de l’applet de commande pour **Get-Help** à l’invite Windows PowerShell pour plus d’informations sur chaque commande.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Cette commande désactive RSS sur la carte réseau que vous spécifiez.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Cette commande permet de RSS sur la carte réseau que vous spécifiez.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Cette commande récupère les propriétés RSS de la carte réseau que vous spécifiez.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Cette commande définit les propriétés RSS sur la carte réseau que vous spécifiez.  

#### <a name="rss-profiles"></a>Profils RSS

Vous pouvez utiliser le **– profil** paramètre de l’applet de commande Set-NetAdapterRss pour spécifier les processeurs logiques sont attribués à la carte réseau. Les valeurs disponibles pour ce paramètre sont:

- **Le plus proche**. Les numéros de processeur logique qui sont proches du processeur de la carte réseau base RSS sont privilégiés. Avec ce profil, le système d’exploitation peut rééquilibrer les processeurs logiques dynamiquement en fonction de charge.
  
- **ClosestStatic**. Les numéros de processeur logique près de processeur RSS de la base de la carte réseau sont par défaut. Avec ce profil, le système d’exploitation ne pas rééquilibrer processeurs logiques dynamiquement en fonction de charge.
  
- **NUMA**. Numéros de processeur logique sont généralement sélectionnés sur différents nœuds NUMA pour distribuer la charge. Avec ce profil, le système d’exploitation peut rééquilibrer les processeurs logiques dynamiquement en fonction de charge.
  
- **NUMAStatic**. Il s’agit du **profil par défaut**. Numéros de processeur logique sont généralement sélectionnés sur différents nœuds NUMA pour distribuer la charge. Avec ce profil, le système d’exploitation ne sera pas rééquilibrer processeurs logiques dynamiquement en fonction de charge.

- **Conservateurs**. RSS utilise moins de processeurs possible pour gérer la charge. Cette option permet de réduire le nombre d’interruptions.

Selon le scénario et les caractéristiques de charge de travail, vous pouvez également utiliser d’autres paramètres de la **Set-NetAdapterRss** applet de commande Windows PowerShell pour spécifier les éléments suivants:

- Chaque carte réseau, le nombre de processeurs logique peut être utilisé pour RSS.
- Le décalage de départ pour la plage de processeurs logiques.
- Le nœud à partir duquel la carte réseau alloue de la mémoire.

Voici les supplémentaires **Set-NetAdapterRss** les paramètres que vous pouvez utiliser pour configurer RSS:

>[!NOTE]
>Dans l’exemple de syntaxe de chaque paramètre ci-dessous, le nom de la carte réseau **Ethernet** est utilisé comme un exemple de valeur pour le **– nom** paramètre de la **Set-NetAdapterRss** commande. Lorsque vous exécutez l’applet de commande, assurez-vous que le nom de la carte réseau que vous utilisez est approprié pour votre environnement.

- **\ * MaxProcessors**: définit le nombre maximal de processeurs RSS à utiliser. Cela garantit que le trafic de l’application est lié à un nombre maximal de processeurs sur une interface donnée. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\ * BaseProcessorGroup**: définit le groupe de processeurs de base d’un nœud NUMA. Le tableau de processeur qui est utilisé par RSS sont affectées. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\ * MaxProcessorGroup**: définit le groupe de processeur Max d’un nœud NUMA. Le tableau de processeur qui est utilisé par RSS sont affectées. Cette définition limiterait un groupe nombre maximal de processeurs afin que l’équilibrage de charge est aligné dans un groupe de k. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\ * BaseProcessorNumber**: définit le nombre de processeurs de base d’un nœud NUMA. Le tableau de processeur qui est utilisé par RSS sont affectées. Cela permet le partitionnement des processeurs sur les cartes réseau. Ceci est le premier processeur dans les processeurs de la plage de RSS logique qui est attribué à chaque carte. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\ * NumaNode**: nœud NUMA le que chaque carte réseau peut allouer de mémoire. Cela peut être dans un groupe de k ou à partir de différents groupes k. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\ * NumberofReceiveQueues**: si vos processeurs logiques semblent être sous-utilisés pour le trafic de réception \ (par exemple, comme indiqué dans le Gestionnaire de tâches fichiers\), vous pouvez essayer d’augmentation du nombre de files d’attente RSS à partir de la valeur par défaut de 2 à la valeur maximale est pris en charge par votre carte réseau. Votre carte réseau est peut-être options permettant de changer le nombre de files d’attente RSS dans le cadre du pilote. Exemple de syntaxe:

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Pour plus d’informations, cliquez sur le lien suivant pour télécharger [mise en réseau évolutive: en éliminant la réception de traitement goulot d’étranglement: présentation de RSS](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) au format Word.
  
#### <a name="understanding-rss-performance"></a>Présentation des performances RSS

Réglage RSS, vous devez comprendre la configuration et la logique d’équilibrage de charge. Pour vérifier que les paramètres RSS ont pris effet, vous pouvez passer en revue la sortie lorsque vous exécutez le **Get-NetAdapterRss** applet de commande Windows PowerShell. Voici un exemple de sortie de cette applet de commande.
  
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

Outre les paramètres qui ont été définies des commandes, l’aspect clé de la sortie est la sortie de table d’indirection. La table d’indirection affiche les compartiments de table de hachage qui sont utilisés pour distribuer le trafic entrant. Dans cet exemple, la notation n:c désigne le K Numa-paire d’index du groupe: processeur qui est utilisé pour diriger le trafic entrant. Nous voir exactement 2 entrées uniques (0:0 et 0:4), qui représentent des groupes k 0/cpu0 k-groupe 0/processeur 4, respectivement.

Il n'existe qu’un seul groupe de k pour ce système (k-groupe 0) et un n (où n < = 128) entrée de table d’indirection. Étant donné que le nombre de files d’attente de réception est défini sur 2, uniquement 2 processeurs (0:0, 0:4) sont choisie, même si le nombre maximal de processeurs est définie à 8. En effet, la table d’indirection est hachage du trafic entrant pour utiliser uniquement les 2 processeurs hors du 8 qui sont disponibles.

Pour exploiter pleinement les processeurs, le nombre de files d’attente de réception RSS doit être égale ou supérieure à processeurs Max. Dans l’exemple précédent, la file d’attente de réception doit être définie à 8 ou supérieur.

#### <a name="nic-teaming-and-rss"></a>Association de cartes réseau et RSS

RSS peut être activé sur une carte réseau qui est associée à une autre carte d’interface réseau à l’aide de l’association de cartes réseau. Dans ce scénario, uniquement la carte réseau physique sous-jacent peut être configurée pour utiliser RSS. Un utilisateur ne peut pas définir des applets de commande RSS sur la carte réseau associée.
  
###  <a name="bkmk_rsc"></a>Recevoir Segment fusion (RSC)

Recevoir \(RSC\) Segment fusion permet de performances en réduisant le nombre d’en-têtes IP qui sont traités pour une quantité de données reçues donnée. Elle doit être utilisée pour aider à l’échelle les performances des données reçues par \(or coalescing\) regroupant les plus petits paquets en unités plus grandes.

Cette approche peut affecter la latence avec des avantages principalement observés dans les gains de débit. RSC est recommandé d’augmenter le débit pour les charges de travail reçus. Envisagez de déployer les cartes réseau qui prennent en charge RSC. 

Sur ces cartes réseau, assurez-vous que RSC \ (il s’agit de la définition\ par défaut), sauf si vous avez des charges de travail spécifiques \ (par exemple, à faible latence, faible débit networking\) qui show bénéficient RSC est éteint.

#### <a name="understanding-rsc-diagnostics"></a>Présentation des Diagnostics RSC

Vous pouvez diagnostiquer RSC à l’aide des applets de commande Windows PowerShell **Get-NetAdapterRsc** et **Get-NetAdapterStatistics**.

Voici la sortie de l’exemple lorsque vous exécutez l’applet de commande Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

Le **obtenir** applet de commande indique si les RSC est activée dans l’interface et si TCP permet RSC se trouver dans un état opérationnel. Le motif de l’échec fournit des détails sur l’échec pour activer RSC sur cette interface.

Dans le scénario précédent, RSC IPv4 est pris en charge et opérationnel dans l’interface. Pour comprendre les échecs de diagnostic, un peut voir fusionnés octets ou exceptions déclenchées. Cela fournit une indication des problèmes de fusion.

Voici la sortie de l’exemple lorsque vous exécutez l’applet de commande Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC et la virtualisation

RSC est uniquement pris en charge dans l’hôte physique lorsque la carte réseau hôte n’est pas liée au commutateur virtuel Hyper-V. RSC est désactivée par le système d’exploitation lorsque l’ordinateur hôte est liée au commutateur virtuel Hyper-V. En outre, les ordinateurs virtuels n’obtiennent pas l’avantage de RSC étant donné que les cartes réseau virtuelles ne prennent pas en charge RSC.

RSC peut être activé pour un ordinateur virtuel lors de la virtualisation d’entrée/sortie racine unique \(SR-IOV\) est activé. Dans ce cas, les fonctions virtuelles prennent en charge la fonctionnalité RSC; Par conséquent, les ordinateurs virtuels également bénéficier des RSC.

##  <a name="bkmk_resources"></a>Ressources de la carte réseau

Plusieurs cartes réseau gérer activement leurs ressources pour obtenir des performances optimales. Plusieurs cartes réseau permettent de configurer manuellement les ressources à l’aide de la **avancée de mise en réseau** onglet de la carte. Pour ces cartes, vous pouvez définir les valeurs d’un nombre de paramètres, y compris le nombre de tampons de réception et envoyer des mémoires tampons.

Configuration des ressources de la carte réseau est simplifiée par l’utilisation des applets de commande Windows PowerShell suivante.

- [Get-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130901.aspx)

- [Set-NetAdapterAdvancedProperty](https://technet.microsoft.com/library/jj130894.aspx)

- [Enable-NetAdapter](https://technet.microsoft.com/library/jj130876.aspx)

- [Enable-NetAdapterBinding](https://technet.microsoft.com/library/jj130913.aspx)

- [Enable-NetAdapterChecksumOffload](https://technet.microsoft.com/library/jj130918.aspx)

- [Enable-NetAdapterIPSecOffload](https://technet.microsoft.com/library/jj130890.aspx)

- [Enable-NetAdapterLso](https://technet.microsoft.com/library/jj130922.aspx)

- [Enable-NetAdapterPowerManagement](https://technet.microsoft.com/library/jj130907.aspx)

- [Enable-NetAdapterQos](https://technet.microsoft.com/library/jj130866.aspx)

- [Enable-NetAdapterRDMA](https://technet.microsoft.com/library/jj130909.aspx)

- [Enable-NetAdapterSriov](https://technet.microsoft.com/library/jj130899.aspx)

Pour plus d’informations, voir [les applets de commande de carte réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Pour obtenir des liens vers toutes les rubriques de ce guide, voir [réglage des performances réseau sous-système](net-sub-performance-top.md).
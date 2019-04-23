---
title: Choix d’une carte réseau
description: Cette rubrique fait partie du guide de réglage de performances du sous-système de réseau pour Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: a6615411-83d9-495f-8a6a-1ebc8b12f164
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2b50f4b286e90a450278243c0294ea0aa7f221bc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875850"
---
# <a name="choosing-a-network-adapter"></a>Choix d’une carte réseau

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette rubrique pour découvrir certaines des fonctionnalités des cartes réseau qui peuvent affecter vos choix d’achat.

Les applications nécessitant des ressources réseau nécessitent des adaptateurs de réseau hautes performances. Cette section explore certaines considérations relatives au choix des cartes réseau, ainsi que comment configurer les paramètres de la carte réseau différente pour obtenir de meilleures performances réseau.

> [!TIP]
>  Vous pouvez configurer les paramètres de carte réseau à l’aide de Windows PowerShell. Pour plus d’informations, consultez [les applets de commande de carte réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

##  <a name="bkmk_offload"></a> Fonctions de déchargement

Déchargement de tâches à partir de l’unité centrale \(processeur\) au réseau de l’adaptateur peut réduire l’utilisation du processeur sur le serveur, ce qui améliore les performances globales du système.

La pile de réseau dans les produits Microsoft peut décharger une ou fonctions de déchargement de davantage de tâches à une carte réseau si vous sélectionnez une carte réseau approprié. Le tableau suivant fournit une brève présentation des capacités de déchargement différents qui sont disponibles dans Windows Server 2016.
  
|Type de déchargement|Description|
|------------------|-----------------|  
|Calcul du checksum pour TCP|La pile réseau peut décharger le calcul et la validation de Transmission Control Protocol \(TCP\) sommes de contrôle sur Envoyer et recevoir des chemins de code. Il peut également décharger le calcul et la validation de IPv4 et IPv6 de sommes de contrôle sur Envoyer et recevoir des chemins de code.|  
|Calcul du checksum pour UDP |La pile réseau peut décharger le calcul et la validation de User Datagram Protocol \(UDP\) sommes de contrôle sur Envoyer et recevoir des chemins de code.|
|Calcul du checksum pour IPv4 |La pile réseau peut décharger le calcul et la validation de IPv4 sommes de contrôle sur Envoyer et recevoir des chemins de code. |
|Calcul du checksum pour IPv6 |La pile réseau peut décharger le calcul et la validation du protocole IPv6, les sommes de contrôle sur Envoyer et recevoir des chemins de code. | 
|Segmentation des paquets TCP volumineux|Prend en charge de la couche de transport TCP/IP v2 Large Send Offload (LSOv2). Avec LSOv2, la couche de transport TCP/IP peut transporter la segmentation des paquets TCP volumineux à la carte réseau.|  
|Trafic entrant \(RSS\)|RSS est une technologie de pilote de réseau qui permet la distribution efficace du réseau de traitement de réception entre plusieurs processeurs dans des systèmes multiprocesseurs. Plus de détails sur RSS est fournie plus loin dans cette rubrique.|  
|Receive Segment Coalescing \(RSC\)|RSC est la possibilité de paquets de groupe pour réduire l’en-tête de traitement qui est nécessaire pour l’hôte pour effectuer. Un maximum de 64 Ko de la charge utile reçue pouvant être fusionné en un seul paquet plus grande pour le traitement. Plus de détails sur RSC est fournie plus loin dans cette rubrique.|  
  
###  <a name="bkmk_rss"></a> Trafic entrant

Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2 et Windows Server 2008 prennent en charge l’échelle côté réception \(RSS\). 

Certains serveurs sont configurés avec plusieurs processeurs logiques partageant des ressources matérielles \(comme un cœur physique\) et qui sont traité comme simultanée multi-Threading \(SMT\) homologues. La technologie Intel Hyper-Threading est un exemple. RSS dirige le traitement du réseau pour atteindre un processeur logique par cœur. Par exemple, sur un serveur avec Hyper-Threading d’Intel, 4 cœurs et 8 processeurs logiques, RSS utilise pas plus de 4 processeurs logiques pour le traitement du réseau.  

RSS distribue les paquets entrants des e/s réseau entre les processeurs logiques afin que les paquets qui appartiennent à la même connexion TCP sont traités sur le même processeur logique, ce qui conserve le classement. 

RSS également charger le trafic de multidiffusion et de monodiffusion UDP de soldes, et il achemine les flux liés \(qui sont déterminé par les adresses source et destination de hachage\) sur le même processeur logique, en conservant l’ordre des arrivées connexes. Cela permet d’améliorer l’évolutivité et performances pour les scénarios exigeants en réception pour les serveurs qui disposent de moins de cartes réseau qu’à les processeurs logiques éligibles. 

#### <a name="configuring-rss"></a>Configuration de RSS

Dans Windows Server 2016, vous pouvez configurer RSS à l’aide des applets de commande Windows PowerShell et les profils RSS. 

Vous pouvez définir les profils RSS à l’aide de la **– profil** paramètre de la **Set-NetAdapterRss** applet de commande Windows PowerShell.

**Commandes Windows PowerShell pour la configuration de RSS**

Les applets de commande suivantes permettent d’afficher et modifier les paramètres RSS par carte réseau.
  
>[!NOTE]
>Pour une référence de commandes détaillées pour chaque applet de commande, y compris la syntaxe et les paramètres, vous pouvez cliquer sur les liens suivants. En outre, vous pouvez passer le nom de l’applet de commande à **Get-Help** à l’invite Windows PowerShell pour plus d’informations sur chaque commande.  

- [Disable-NetAdapterRss](https://technet.microsoft.com/library/jj130892). Cette commande désactive RSS sur la carte réseau que vous spécifiez.

- [Enable-NetAdapterRss](https://technet.microsoft.com/library/jj130859). Cette commande active RSS sur la carte réseau que vous spécifiez.
  
- [Get-NetAdapterRss](https://technet.microsoft.com/library/jj130912). Cette commande récupère les propriétés RSS de la carte réseau que vous spécifiez.
  
- [Set-NetAdapterRss](https://technet.microsoft.com/library/jj130863). Cette commande définit les propriétés RSS sur la carte réseau que vous spécifiez.  

#### <a name="rss-profiles"></a>Profils RSS

Vous pouvez utiliser la **– profil** paramètre de l’applet de commande Set-NetAdapterRss pour spécifier quels processeurs logiques sont affectés à la carte réseau. Les valeurs disponibles pour ce paramètre sont :

- **Le plus proche**. Numéros des processeurs logiques qui approchent de processeur RSS de la base de la carte réseau sont préférables. Avec ce profil, le système d’exploitation peut rééquilibrer les processeurs logiques dynamiquement en fonction de charge.
  
- **ClosestStatic**. Numéros des processeurs logiques près de processeur RSS de la base de la carte réseau sont préférables. Avec ce profil, le système d’exploitation ne pas rééquilibrer les processeurs logiques dynamiquement en fonction de charge.
  
- **NUMA**. Numéros des processeurs logiques sont généralement sélectionnés sur différents nœuds NUMA pour répartir la charge. Avec ce profil, le système d’exploitation peut rééquilibrer les processeurs logiques dynamiquement en fonction de charge.
  
- **NUMAStatic**. Il s’agit du **profil par défaut**. Numéros des processeurs logiques sont généralement sélectionnés sur différents nœuds NUMA pour répartir la charge. Avec ce profil, le système d’exploitation ne sera pas rééquilibrer dynamiquement en fonction de charge de processeurs logiques.

- **Conservatrice**. RSS utilise moins de processeurs possible pour gérer la charge. Cette option permet de réduire le nombre d’interruptions.

Selon le scénario et les caractéristiques de charge de travail, vous pouvez également utiliser d’autres paramètres de la **Set-NetAdapterRss** applet de commande Windows PowerShell pour spécifier les éléments suivants :

- Sur une base de carte réseau, le nombre de processeurs logique utilisable pour RSS.
- Décalage de départ pour la plage de processeurs logiques.
- Le nœud à partir de laquelle la carte réseau alloue de la mémoire.

Voici le supplémentaires **Set-NetAdapterRss** les paramètres que vous pouvez utiliser pour configurer RSS :

>[!NOTE]
>Dans l’exemple de syntaxe pour chaque paramètre ci-dessous, le nom de l’adaptateur réseau **Ethernet** est utilisé comme un exemple de valeur pour le **– nom** paramètre de la **Set-NetAdapterRss** commande. Lorsque vous exécutez l’applet de commande, assurez-vous que le nom de la carte réseau que vous utilisez est approprié pour votre environnement.

- **\* MaxProcessors**: Définit le nombre maximal de processeurs RSS à utiliser. Cela garantit que le trafic d’application est lié à un nombre maximal de processeurs sur une interface donnée. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessors <value>`

- **\* BaseProcessorGroup**: Définit le groupe de processeurs de base d’un nœud NUMA. Ceci influe sur le tableau de processeur qui est utilisé par RSS. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorGroup <value>`
  
- **\* MaxProcessorGroup**: Définit le groupe de processeur maximale d’un nœud NUMA. Ceci influe sur le tableau de processeur qui est utilisé par RSS. Définition de cette propriété limiterait un groupe de processeur maximal afin que l’équilibrage de charge est aligné dans un groupe de k. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –MaxProcessorGroup <value>`

- **\* BaseProcessorNumber**: Définit le nombre de processeurs de base d’un nœud NUMA. Ceci influe sur le tableau de processeur qui est utilisé par RSS. Cela permet le partitionnement de processeurs sur les cartes réseau. Ceci est le premier processeur dans les processeurs de la plage de RSS logique qui est attribué à chaque carte. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –BaseProcessorNumber <Byte Value>`

- **\* NumaNode**: Le nœud NUMA que chaque carte réseau peut allouer de mémoire. Cela peut être dans un groupe de k ou à partir de différents k-groups. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –NumaNodeID <value>`

- **\* NumberofReceiveQueues**: Si vos processeurs logiques semblent sont sous-utilisés pour le trafic de réception \(, par exemple, comme affiché dans le Gestionnaire des tâches\), vous pouvez essayer d’augmenter le nombre de files d’attente RSS à partir de la valeur par défaut de 2 à la valeur maximale qui est pris en charge par votre carte réseau . Votre carte réseau peut avoir des options pour modifier le nombre de files d’attente RSS dans le cadre du pilote. Exemple de syntaxe :

     `Set-NetAdapterRss –Name “Ethernet” –NumberOfReceiveQueues <value>`

Pour plus d’informations, cliquez sur le lien suivant pour télécharger [mise en réseau évolutive : Élimination de la réception de traitement goulot d’étranglement — RSS présentation](https://download.microsoft.com/download/5/D/6/5D6EAF2B-7DDF-476B-93DC-7CF0072878E6/NDIS_RSS.doc) au format Word.
  
#### <a name="understanding-rss-performance"></a>Comprendre les performances de RSS

Réglage de RSS, vous devez comprendre la configuration et la logique de l’équilibrage de charge. Pour vérifier que les paramètres RSS ont pris effet, vous pouvez consulter la sortie lorsque vous exécutez le **Get-NetAdapterRss** applet de commande Windows PowerShell. Voici un exemple de sortie de cette applet de commande.
  
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

Outre reproduit en écho les paramètres qui ont été définies, l’aspect clé de la sortie est la sortie de table d’indirection. La table d’indirection affiche les compartiments de table de hachage qui servent à distribuer le trafic entrant. Dans cet exemple, la notation n:c désigne le Numa K-paire d’index du groupe : processeur qui sert à diriger le trafic entrant. Nous voir exactement 2 entrées uniques (0:0 et 0:4), qui représentent des k-group 0/cpu0 k-group 0/UC 4, respectivement.

Il n'existe qu’un seul groupe de k pour ce système (k-group 0) et un n (où n < = 128) entrée de table d’indirection. Étant donné que le nombre de files d’attente de réception est défini sur 2, uniquement 2 processeurs (0:0, 0:4) sont choisis - même si le nombre maximal de processeurs est défini sur 8. En effet, la table d’indirection est hachage du trafic entrant pour utiliser uniquement 2 processeurs hors 8 qui sont disponibles.

Pour utiliser pleinement l’UC, le nombre de files d’attente de réception RSS doit être égale ou supérieure à processeurs de Max. Dans l’exemple précédent, la file d’attente de réception doit être définie à 8 ou supérieur.

#### <a name="nic-teaming-and-rss"></a>Association de cartes réseau et les flux RSS

RSS peut être activé sur une carte réseau qui est associée à une autre carte d’interface réseau à l’aide d’association de cartes réseau. Dans ce scénario, uniquement la carte réseau physique sous-jacent peut être configurée pour utiliser RSS. Un utilisateur ne peut pas définir des applets de commande RSS sur la carte réseau associée.
  
###  <a name="bkmk_rsc"></a> Receive Segment Coalescing (RSC)

Receive Segment Coalescing \(RSC\) permet des performances en réduisant le nombre d’en-têtes d’adresse IP qui sont traités pendant une durée donnée des données reçues. Il doit être utilisé pour aider à adapter les performances des données reçues en regroupant les \(ou fusion\) les paquets plus petits en unités plus importantes.

Cette approche peut affecter la latence avec avantages constatés principalement dans les gains de débit. RSC est recommandé d’augmenter le débit pour les charges de travail reçus. Envisagez de déployer des cartes réseau qui prennent en charge RSC. 

Sur ces cartes réseau, assurez-vous que RSC est activé \(il s’agit du paramètre par défaut\), sauf si vous avez des charges de travail spécifiques \(par exemple, une latence faible, mise en réseau de débit à faible\) cet avantage afficher à partir de la désactivation de RSC .

#### <a name="understanding-rsc-diagnostics"></a>Fonctionnement des Diagnostics RSC

Vous pouvez diagnostiquer RSC à l’aide des applets de commande Windows PowerShell **Get-NetAdapterRsc** et **Get-NetAdapterStatistics**.

Voici exemple de sortie lorsque vous exécutez l’applet de commande Get-NetAdapterRsc.

```  

PS C:\Users\Administrator> Get-NetAdapterRsc  
  
Name                       IPv4Enabled  IPv6Enabled  IPv4Operational IPv6Operational               IPv4FailureReason              IPv6Failure  
                                            Reason  
----                           -----------  -----------  --------------- --------------- ----------------- ------------  
Ethernet                       True         False        True            False                  NoFailure       NicProperties  
  
```  

Le **obtenir** applet de commande indique si les RSC est activé dans l’interface et indique si TCP permet de RSC se trouver dans un état opérationnel. La raison de l’échec fournit des détails sur l’échec pour activer RSC sur cette interface.

Dans le scénario précédent, IPv4 RSC est prise en charge opérationnelle dans l’interface. Pour comprendre les défaillances de diagnostics, vous pouvez voir les octets fusionnés ou les exceptions provoquées. Cela fournit une indication des problèmes de fusion.

Voici exemple de sortie lorsque vous exécutez l’applet de commande Get-NetAdapterStatistics.

```  
PS C:\Users\Administrator> $x = Get-NetAdapterStatistics “myAdapter”   
PS C:\Users\Administrator> $x.rscstatistics  
  
CoalescedBytes       : 0  
CoalescedPackets     : 0  
CoalescingEvents     : 0  
CoalescingExceptions : 0  
  
```  

#### <a name="rsc-and-virtualization"></a>RSC et virtualisation

RSC est uniquement pris en charge dans l’hôte physique lors de la carte réseau hôte n’est pas liée au commutateur virtuel Hyper-V. RSC est désactivée par le système d’exploitation lors de l’hôte est liée au commutateur virtuel Hyper-V. En outre, les machines virtuelles n’obtenez pas l’avantage de RSC étant donné que les cartes réseau virtuelles ne prennent pas en charge RSC.

RSC peut être activée pour une machine virtuelle lors de la virtualisation d’une racine unique d’entrée/sortie \(SR-IOV\) est activé. Dans ce cas, les fonctions virtuelles prennent en charge la fonctionnalité RSC ; Par conséquent, les machines virtuelles reçoivent également l’avantage de RSC.

##  <a name="bkmk_resources"></a> Ressources de la carte réseau

Plusieurs cartes réseau gérer activement leurs ressources pour optimiser les performances. Plusieurs cartes réseau permettent de configurer manuellement les ressources à l’aide de la **Advanced Networking** onglet de l’adaptateur. Pour ces adaptateurs, vous pouvez définir les valeurs d’un nombre de paramètres, y compris le nombre de tampons de réception et les mémoires tampons d’envoi.

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

Pour plus d’informations, consultez [les applets de commande de carte réseau dans Windows PowerShell](https://technet.microsoft.com/library/jj134956.aspx).

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances réseau sous-système](net-sub-performance-top.md).
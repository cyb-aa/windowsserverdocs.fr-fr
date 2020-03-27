---
title: Réglage des performances des cartes réseau
description: Cette rubrique fait partie du Guide d’optimisation des performances du sous-système réseau pour Windows Server 2016.
audience: Admin
ms.custom:
- CI ID 111485
- CSSTroubleshoot
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0b9b0f80-415c-4f5e-8377-c09b51d9c5dd
manager: dcscontentpm
ms.author: lizross
author: Teresa-Motiv
ms.date: 12/23/2019
ms.openlocfilehash: f802804d64b3047a2612b7f346de03aff61c30cd
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316539"
---
# <a name="performance-tuning-network-adapters"></a>Réglage des performances des cartes réseau

> S'applique à : Windows Server 2019, Windows Server 2016, Windows Server (canal semi-annuel)

Utilisez les informations de cette rubrique pour régler les cartes réseau de performances pour les ordinateurs qui exécutent Windows Server 2016 et versions ultérieures. Si vos cartes réseau fournissent des options de paramétrage, vous pouvez utiliser ces options pour optimiser le débit du réseau et l’utilisation des ressources.

Les paramètres de réglage corrects pour vos cartes réseau dépendent des variables suivantes :

- La carte réseau et son jeu de fonctionnalités  
- Le type de charge de travail effectué par le serveur  
- Les ressources matérielles et logicielles du serveur  
- Vos objectifs de performances pour le serveur  

Les sections qui suivent décrivent certaines options de réglage des performances.  

##  <a name="enabling-offload-features"></a><a name="bkmk_offload"></a>Activation des fonctionnalités de déchargement

L'activation des fonctionnalités de déchargement de la carte réseau est souvent bénéfique. Toutefois, la carte réseau n’est peut-être pas suffisamment puissante pour gérer les capacités de déchargement avec un débit élevé.

> [!IMPORTANT]
> N’utilisez pas les fonctionnalités de déchargement déchargement de **tâche IPSec** ou **déchargement TCP Chimney**. Ces technologies sont déconseillées dans Windows Server 2016 et peuvent nuire aux performances du serveur et de la mise en réseau. En outre, ces technologies peuvent ne pas être prises en charge par Microsoft à l’avenir.

Par exemple, considérez une carte réseau avec des ressources matérielles limitées.
Dans ce cas, l’activation des fonctionnalités de déchargement de segment peut réduire le débit maximal viable de l’adaptateur. Toutefois, si le débit réduit est acceptable, vous devez commencer par activer les fonctionnalités de déchargement de segmentation.

> [!NOTE]  
> Certaines cartes réseau nécessitent que vous activiez les fonctionnalités de déchargement indépendamment pour les chemins d’envoi et de réception.

##  <a name="enabling-receive-side-scaling-rss-for-web-servers"></a><a name="bkmk_rss_web"></a>Activation de la mise à l’échelle côté réception (RSS) pour les serveurs Web

RSS peut améliorer l'extensibilité web et les performances lorsque le serveur comprend moins de cartes réseau que de processeurs logiques. Lorsque l’ensemble du trafic Web passe par les cartes réseau capables d’utiliser RSS, le serveur peut traiter les demandes Web entrantes à partir de différentes connexions simultanément entre différents processeurs.

> [!IMPORTANT]  
> Évitez d’utiliser à la fois des cartes réseau non RSS et des cartes réseau avec compatibilité RSS sur le même serveur. En raison de la logique de distribution de charge dans RSS et HTTP (Hypertext Transfer Protocol), les performances peuvent se dégrader gravement si une carte réseau non basée sur RSS accepte le trafic Web sur un serveur qui possède une ou plusieurs cartes réseau prenant en charge RSS. Dans cette situation, vous devez utiliser les cartes prenant en charge RSS ou désactiver RSS sous l'onglet **Propriétés avancées** dans les propriétés de la carte réseau.
>  
> Pour déterminer si une carte réseau prend en charge RSS, vous pouvez consulter l'onglet **Propriétés avancées** dans les propriétés de la carte réseau.

### <a name="rss-profiles-and-rss-queues"></a>Profils RSS et Files d'attente RSS

Le profil prédéfini RSS par défaut est **NUMAStatic**, qui diffère de la valeur par défaut utilisée par les versions précédentes de Windows. Avant de commencer à utiliser des profils RSS, passez en revue les profils disponibles pour savoir quand ils sont bénéfiques et comment ils s’appliquent à votre environnement réseau et à votre matériel.

Par exemple, si vous ouvrez le gestionnaire des tâches et passez en revue les processeurs logiques sur votre serveur et qu’ils semblent être sous-utilisés pour le trafic de réception, vous pouvez essayer d’élever le nombre de files d’attente RSS de la valeur par défaut de deux à la valeur maximale prise en charge par votre carte réseau. Votre carte réseau peut proposer des options permettant de changer le nombre de files d'attente RSS dans le pilote.

##  <a name="increasing-network-adapter-resources"></a><a name="bkmk_resources"></a>Amélioration des ressources de la carte réseau

Pour les cartes réseau qui vous permettent de configurer manuellement des ressources telles que des tampons de réception et d’envoi, vous devez augmenter les ressources allouées.  

Certaines cartes réseau définissent des tampons de réception faibles afin de conserver la mémoire allouée depuis l'hôte. Cela conduit à des paquets ignorés et des performances réduites. Par conséquent, dans les scénarios de réception intensive, nous vous recommandons d'augmenter la valeur du tampon de réception à son maximum.

> [!NOTE]  
> Si une carte réseau n’expose pas la configuration manuelle des ressources, elle configure dynamiquement les ressources ou les ressources sont définies sur une valeur fixe qui ne peut pas être modifiée.

### <a name="enabling-interrupt-moderation"></a>Activation de la modération des interruptions

Pour contrôler la modération des interruptions, certaines cartes réseau exposent différents niveaux de modération des interruptions, différents paramètres de fusion de tampons (parfois séparément pour les mémoires tampons d’envoi et de réception), ou les deux.

Vous devez envisager la modération des interruptions pour les charges de travail liées au processeur. Lors de l’utilisation de la modération des interruptions, envisagez le compromis entre les économies de l’UC de l’ordinateur hôte et la latence par rapport à l’augmentation du nombre d’interruptions de l’ordinateur hôte en raison de plus d’interruptions et d’une latence Si la carte réseau n’effectue pas de modération d’interruption, mais qu’elle expose la fusion de tampons, vous pouvez améliorer les performances en accroissant le nombre de mémoires tampons fusionnées pour permettre davantage de mémoires tampons par envoi ou par réception.

##  <a name="performance-tuning-for-low-latency-packet-processing"></a><a name="bkmk_low"></a>Réglage des performances pour le traitement des paquets à faible latence

De nombreuses cartes réseaux proposent des options permettant d'optimiser la latence induite par le système d'exploitation. La latence est la durée qui sépare le traitement d'un paquet entrant par le pilote réseau et le renvoi du paquet par ce pilote. Cette durée est généralement mesurée en microsecondes. À titre de comparaison, la durée de transmission de paquets sur de longues distances est généralement mesurée en millisecondes (un ordre de magnitude plus grand). Ce réglage ne réduira pas la durée de transit d'un paquet.

Voici quelques suggestions de réglage des performances pour les réseaux sensibles à la microseconde.

- Définissez le BIOS de l'ordinateur sur **Performances élevées** et désactivez les états C. Notez toutefois que cela dépend du système et du BIOS et que les performances de certains systèmes seront meilleures si le système d'exploitation contrôle la gestion de l'alimentation. Vous pouvez vérifier et ajuster vos paramètres de gestion de l’alimentation à partir de **paramètres** ou à l’aide de la commande **powercfg** . Pour plus d’informations, consultez [options de ligne de commande powercfg](https://docs.microsoft.com/windows-hardware/design/device-experiences/powercfg-command-line-options).

- Définissez le profil de gestion de l'alimentation du système d'exploitation sur **Système hautes performances**.  
   > [!NOTE]  
   > Ce paramètre ne fonctionne pas correctement si le BIOS du système a été défini pour désactiver le contrôle du système d’exploitation de la gestion de l’alimentation.

- Activez les déchargements statiques. Par exemple, activez les sommes de contrôle UDP, les sommes de contrôle TCP et les paramètres d’envoi de déchargement volumineux (LSO).

- Si le trafic est multidiffusé, par exemple lors de la réception d’un trafic de multidiffusion à volume élevé, activez RSS.

- Désactivez le paramètre **Modération de l'interruption** pour les pilotes de carte réseau qui requièrent la latence la plus faible possible. N’oubliez pas que cette configuration peut utiliser plus de temps processeur et représente un compromis.

- Gérez les interruptions de carte réseau et les appels de procédure différés (DPC) sur un processeur cœur qui partage le cache d'UC avec le cœur qui est utilisé par le programme (thread utilisateur) qui traite le paquet. Le réglage de l'affinité de l'UC peut être utilisé pour diriger un processus vers certains processeurs logiques, conjointement avec la configuration RSS. L'utilisation du même cœur pour le thread d'interruption, le thread DPC et le thread de mode utilisateur réduit les performances à mesure que la charge augmente du fait de la compétition entre ISR, DPC et le thread pour utiliser le cœur.

##  <a name="system-management-interrupts"></a><a name="bkmk_smi"></a>Interruptions de gestion du système

De nombreux systèmes matériels utilisent des interruptions de gestion du système (SMI) pour diverses fonctions de maintenance, telles que la signalisation des erreurs de mémoire ECC, la gestion de la compatibilité USB existante, le contrôle du ventilateur et la gestion de l’alimentation contrôlée par le BIOS. Paramètres.

SMI est l’interruption de priorité la plus élevée sur le système et place le processeur en mode de gestion. Ce mode anticipe toutes les autres activités pendant que SMI exécute une routine de service d’interruption, généralement contenue dans le BIOS.

Malheureusement, ce comportement peut entraîner des pics de latence de 100 microsecondes ou plus.

Si vous avez besoin d'une latence très faible, demandez à votre fournisseur de matériel une version du BIOS qui réduit au maximum les SMI. Ces versions du BIOS sont souvent appelées « BIOS à faible latence » ou « BIOS gratuit SMI ». Dans certains cas, une plateforme matérielle ne peut pas complètement éliminer l'activité SMI car elle est utilisée pour contrôler des fonctions essentielles (ventilateurs de refroidissement par exemple).

> [!NOTE]  
> Le système d’exploitation ne peut pas contrôler SMIs, car le processeur logique s’exécute en mode de maintenance spécial, ce qui empêche l’intervention du système d’exploitation.

##  <a name="performance-tuning-tcp"></a><a name="bkmk_tcp"></a>Réglage des performances TCP

 Vous pouvez utiliser les éléments suivants pour régler les performances TCP.

###  <a name="tcp-receive-window-autotuning"></a><a name="bkmk_tcp_params"></a>Réglage automatique de la fenêtre de réception TCP

Dans Windows Vista, Windows Server 2008 et les versions ultérieures de Windows, la pile réseau Windows utilise une fonctionnalité nommée niveau de réglage automatique de la *fenêtre de réception TCP* pour négocier la taille de la fenêtre de réception TCP. Cette fonctionnalité peut négocier une taille de fenêtre de réception définie pour chaque communication TCP pendant la négociation TCP.

Dans les versions antérieures de Windows, la pile réseau Windows utilisait une fenêtre de réception de taille fixe (65 535 octets) qui limitait le débit potentiel global pour les connexions. Le débit total réalisable des connexions TCP peut limiter les scénarios d’utilisation du réseau. Le réglage automatique de la fenêtre de réception TCP permet à ces scénarios d’utiliser pleinement le réseau.

Pour une fenêtre de réception TCP qui a une taille particulière, vous pouvez utiliser l’équation suivante pour calculer le débit total d’une connexion unique.

> *Débit total réalisable en octets* = *taille de la fenêtre de réception TCP en octets* \* (1/ *latence de connexion en secondes*)

Par exemple, pour une connexion dont la latence est de 10 ms, le débit total réalisable est uniquement de 51 Mbits/s. Cette valeur est raisonnable pour une grande infrastructure réseau d’entreprise. Toutefois, en utilisant la fonction de réglage automatique pour ajuster la fenêtre de réception, la connexion peut atteindre le débit total d’une connexion de 1 Gbit/s.  

Certaines applications définissent la taille de la fenêtre de réception TCP. Si l’application ne définit pas la taille de la fenêtre de réception, la vitesse de liaison détermine la taille comme suit :

- Moins de 1 mégabit par seconde (Mbits/s) : 8 kilo-octets (Ko)
- 1 Mbits/s à 100 Mbits/s : 17 Ko
- 100 Mbits/s à 10 gigabits par seconde (Gbits/s) : 64 Ko
- 10 Gbit/s ou plus : 128 Ko

Par exemple, sur un ordinateur sur lequel est installée une carte réseau de 1 Gbit/s, la taille de la fenêtre doit être de 64 Ko.

Cette fonctionnalité permet également d’utiliser pleinement d’autres fonctionnalités pour améliorer les performances du réseau. Ces fonctionnalités incluent le reste des options TCP définies dans le [document RFC 1323](https://tools.ietf.org/html/rfc1323). En utilisant ces fonctionnalités, les ordinateurs Windows peuvent négocier des tailles de fenêtre de réception TCP plus petites, mais mises à l’échelle à une valeur définie, en fonction de la configuration. Ce comportement est plus facile à gérer pour les périphériques réseau.

> [!NOTE]  
> Vous pouvez rencontrer un problème dans lequel le périphérique réseau n’est pas conforme à l’option de mise à l’échelle de la **fenêtre TCP**, comme défini dans la [norme RFC 1323](https://tools.ietf.org/html/rfc1323) et, par conséquent, ne prend pas en charge le facteur d’échelle. Dans ce cas, reportez-vous à la [base de connaissances KB 934430, la connectivité réseau échoue lorsque vous essayez d’utiliser Windows Vista derrière un pare-feu](https://support.microsoft.com/help/934430/network-connectivity-fails-when-you-try-to-use-windows-vista-behind-a) ou contactez l’équipe de support technique de votre fournisseur de périphériques réseau.  

#### <a name="review-and-configure-tcp-receive-window-autotuning-level"></a>Vérifier et configurer le niveau de réglage automatique de la fenêtre de réception TCP

Vous pouvez utiliser des commandes netsh ou des applets de commande Windows PowerShell pour vérifier ou modifier le niveau de réglage automatique de la fenêtre de réception TCP.

> [!NOTE]  
> Contrairement aux versions de Windows antérieures à Windows 10 ou Windows Server 2019, vous ne pouvez plus utiliser le registre pour configurer la taille de la fenêtre de réception TCP. Pour plus d’informations sur les paramètres déconseillés, consultez [paramètres TCP déconseillés](#deprecated-tcp-parameters).

> [!NOTE]  
> Pour plus d’informations sur les niveaux de réglage automatique disponibles, consultez [niveaux de réglage](#autotuning-levels)automatique.

**Pour utiliser Netsh pour examiner ou modifier le niveau de réglage automatique**

Pour vérifier les paramètres actuels, ouvrez une fenêtre d’invite de commandes et exécutez la commande suivante :

```cmd
netsh interface tcp show global
```

La sortie de cette commande doit ressembler à ce qui suit :

```
Querying active state...

TCP Global Parameters  
-----
Receive-Side Scaling State : enabled
Chimney Offload State : disabled
Receive Window Auto-Tuning Level : normal
Add-On Congestion Control Provider : default
ECN Capability : disabled
RFC 1323 Timestamps : disabled
Initial RTO : 3000
Receive Segment Coalescing State : enabled
Non Sack Rtt Resiliency : disabled
Max SYN Retransmissions : 2
Fast Open : enabled
Fast Open Fallback : enabled
Pacing Profile : off
```

Pour modifier le paramètre, exécutez la commande suivante à l’invite de commandes :

```cmd
netsh interface tcp set global autotuninglevel=<Value>
```

> [!NOTE]  
> Dans la commande précédente, \<*valeur*> représente la nouvelle valeur pour le niveau de réglage automatique.

Pour plus d’informations sur cette commande, consultez [commandes netsh pour le protocole interface Transmission Control Protocol](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731258(v=ws.10)).

**Pour utiliser PowerShell pour vérifier ou modifier le niveau de réglage automatique**

Pour passer en revue les paramètres actuels, ouvrez une fenêtre PowerShell et exécutez l’applet de commande suivante.

```PowerShell
Get-NetTCPSetting | Select SettingName,AutoTuningLevelLocal
```

La sortie de cette applet de commande doit ressembler à ce qui suit.

```
SettingName           AutoTuningLevelLocal
-----------          --------------------
Automatic
InternetCustom       Normal
DatacenterCustom     Normal
Compat               Normal
Datacenter           Normal
Internet             Normal
```

Pour modifier le paramètre, exécutez l’applet de commande suivante à l’invite de commandes PowerShell.

```PowerShell
Set-NetTCPSetting -AutoTuningLevelLocal <Value>
```

> [!NOTE]  
> Dans la commande précédente, \<*valeur*> représente la nouvelle valeur pour le niveau de réglage automatique.

Pour plus d’informations sur ces applets de commande, consultez les articles suivants :

- [NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/get-nettcpsetting?view=win10-ps)
- [Set-NetTCPSetting](https://docs.microsoft.com/powershell/module/nettcpip/set-nettcpsetting?view=win10-ps)

#### <a name="autotuning-levels"></a>Niveaux de réglage automatique

Vous pouvez définir le réglage automatique de la fenêtre de réception sur l’un des cinq niveaux. Le niveau par défaut est **normal**. Le tableau suivant décrit les niveaux.

|Level |Valeur hexadécimale |Commentaires |
| --- | --- | --- |
|Normale (par défaut) |0x8 (facteur d’échelle de 8) |Définissez la taille de la fenêtre de réception TCP pour s’adapter à presque tous les scénarios. |
|Désactivé |Aucun facteur d’échelle disponible |Définissez la valeur par défaut de la fenêtre de réception TCP. |
|Restricted (Restreint) |0x4 (facteur d’échelle de 4) |Définissez la taille de la fenêtre de réception TCP au-delà de sa valeur par défaut, mais Limitez cette croissance dans certains scénarios. |
|Hautement restreint |0X2 (facteur d’échelle de 2) |Définissez la taille de la fenêtre de réception TCP au-delà de sa valeur par défaut, mais faites-le très prudentment. |
|pratiqué |0xE (facteur d’échelle de 14) |Définissez la taille de la fenêtre de réception TCP pour s’adapter aux scénarios extrêmes. |

Si vous utilisez une application pour capturer des paquets réseau, l’application doit signaler les données qui ressemblent à ce qui suit pour les différents paramètres de niveau de réglage automatique de la fenêtre.

- Niveau de réglage automatique : **normal (État par défaut)**

   ```
   Frame: Number = 492, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2667, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60975, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=4075590425, Ack=0, Win=64240 ( Negotiating scale factor 0x8 ) = 64240
   SrcPort: 60975
   DstPort: Microsoft-DS(445)
   SequenceNumber: 4075590425 (0xF2EC9319)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x8 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x8 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 8 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Niveau de réglage automatique : **désactivé**

   ```
   Frame: Number = 353, Captured Frame Length = 62, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2576, Total IP Length = 48
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60956, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2315885330, Ack=0, Win=64240 ( ) = 64240
   SrcPort: 60956
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2315885330 (0x8A099B12)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 112 (0x70)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( ) = 64240 ----------------------------------------> TCP Receive Window set as 64K as per NIC Link bitrate. Note there is no Scale Factor defined. In this case, Scale factor is not being sent as a TCP Option, so it will not be used by Windows.
   Checksum: 0x817E, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Niveau de réglage automatique : **limité**

   ```
   Frame: Number = 3, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2319, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60890, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1966088568, Ack=0, Win=64240 ( Negotiating scale factor 0x4 ) = 64240
   SrcPort: 60890
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1966088568 (0x75302178)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x4 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x4 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 4 -------------------------------> Scale factor, defined by AutoTuningLevel.
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Niveau de réglage automatique : **hautement restreint**

   ```
   Frame: Number = 115, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2388, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60903, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=1463725706, Ack=0, Win=64240 ( Negotiating scale factor 0x2 ) = 64240
   SrcPort: 60903
   DstPort: Microsoft-DS(445)
   SequenceNumber: 1463725706 (0x573EAE8A)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0x2 ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0x2 Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 2 ------------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

- Niveau de réglage automatique : **expérimental**

   ```
   Frame: Number = 238, Captured Frame Length = 66, MediaType = ETHERNET
   + Ethernet: Etype = Internet IP (IPv4),DestinationAddress:[D8-FE-E3-65-F3-FD],SourceAddress:[C8-5B-76-7D-FA-7F]
   + Ipv4: Src = 192.169.0.5, Dest = 192.169.0.4, Next Protocol = TCP, Packet ID = 2490, Total IP Length = 52
   - Tcp: [Bad CheckSum]Flags=......S., SrcPort=60933, DstPort=Microsoft-DS(445), PayloadLen=0, Seq=2095111365, Ack=0, Win=64240 ( Negotiating scale factor 0xe ) = 64240
   SrcPort: 60933
   DstPort: Microsoft-DS(445)
   SequenceNumber: 2095111365 (0x7CE0DCC5)
   AcknowledgementNumber: 0 (0x0)
   + DataOffset: 128 (0x80)
   + Flags: ......S. ---------------------------------------------------------> SYN Flag set
   Window: 64240 ( Negotiating scale factor 0xe ) = 64240 ---------> TCP Receive Window set as 64K as per NIC Link bitrate. Note it shows the 0xe Scale Factor.
   Checksum: 0x8182, Bad
   UrgentPointer: 0 (0x0)
   - TCPOptions:
   + MaxSegmentSize: 1
   + NoOption:
   + WindowsScaleFactor: ShiftCount: 14 -----------------------------> Scale factor, defined by AutoTuningLevel
   + NoOption:
   + NoOption:
   + SACKPermitted:
   ```

#### <a name="deprecated-tcp-parameters"></a>Paramètres TCP déconseillés

Les paramètres de registre suivants de Windows Server 2003 ne sont plus pris en charge et sont ignorés dans les versions ultérieures.

- **TcpWindowSize**
- **NumTcbTablePartitions**  
- **MaxHashTableSize**  

Tous ces paramètres se trouvaient dans la sous-clé de Registre suivante :

> **HKEY_LOCAL_MACHINE \System\CurrentControlSet\Services\Tcpip\Parameters**  

###  <a name="windows-filtering-platform"></a><a name="bkmk_wfp"></a>Plateforme de filtrage Windows

Windows Vista et Windows Server 2008 ont introduit la plateforme de filtrage Windows (WFP). WFP fournit des API aux éditeurs de logiciels indépendants non-Microsoft (ISV) pour créer des filtres de traitement de paquets. Les logiciels pare-feu et antivirus en sont des exemples.

> [!NOTE]  
> Un filtre WFP mal écrit peut réduire considérablement les performances réseau d’un serveur. Pour plus d’informations, consultez [Portage de pilotes et d’applications de traitement de paquets vers WFP](https://docs.microsoft.com/windows-hardware/drivers/network/porting-packet-processing-drivers-and-apps-to-wfp) dans le centre de développement Windows.

Pour obtenir des liens vers toutes les rubriques de ce guide, consultez [réglage des performances du sous-système réseau](net-sub-performance-top.md).
